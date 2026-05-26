# 2026 (ongoing) — Hooks and the effort knob

## Why this matters

Hooks are the deterministic-enforcement layer of the three-layer
extensibility stack (the other two: skills + subagents). They run
*outside* the LLM, on your machine, in response to events the
Claude Code harness emits. If a prompt asks for behaviour and the
LLM might or might not comply, a hook either compels behaviour or
blocks the action that would violate it. That asymmetry is what
makes hooks load-bearing for any team treating Claude Code as
infrastructure.

2026 added several hook-shape changes that aren't huge individually
but cumulatively are the difference between "this works on my
laptop" and "this is something you'd actually deploy as a team
standard." The headline addition is the **effort knob** — a
session-wide "how hard should the model try" setting that flows
into hook input. The other 2026 changes mostly cluster around
making hooks robust (the `Stop` 8-consecutive-block cap) and
observable (`duration_ms`, `terminalSequence`).

## The verified 2026 hook event list

Hook events as documented in the [Anthropic
changelog](https://code.claude.com/docs/en/changelog) and visible in
real `~/.claude/settings.json` files in the wild:

| Event | When it fires | Notes |
|---|---|---|
| `SessionStart` | New Claude Code session begins | The most common hook target. Fires *also* on session compaction with a `compact` matcher you can filter on. |
| `Setup` | Initial harness setup, before the first user turn | Use for one-shot environment preparation; runs once per session, before `SessionStart` in some configurations. |
| `SubagentStart` | A subagent is dispatched (Task tool) | Per-dispatch; useful for observability. |
| `SubagentStop` | A subagent finishes | Per-dispatch. Pair with `SubagentStart` for full-dispatch metrics. |
| `Stop` | The main agent's turn ends (the model stops generating) | Blocking via exit code 2 is *the* enforcement lever. **New in 2.1.143: caps at 8 consecutive blocks** to prevent infinite re-blocking. |
| `UserPromptSubmit` | User submits a turn to the model | Pre-flight check on user input. |
| `PreToolUse` | Before a tool call executes | Block via exit code 2 to refuse a tool call. The main authorisation hook. |
| `PostToolUse` | After a tool call returns success | **`duration_ms` added in 2.1.119** (excludes permission-prompt time + PreToolUse hook time). |
| `PostToolUseFailure` | After a tool call returns failure | Separate from `PostToolUse` so you can react to failures specifically. Lets you build self-healing flows without re-implementing failure detection. |
| `PermissionRequest` | A tool call would trigger a permission prompt | Lets you auto-approve or auto-deny based on context, bypassing the interactive prompt. |
| `Notification` | Claude wants to surface something to the user | Useful for desktop-bell / window-title integration via `terminalSequence` (see below). |
| `PreCompact` | Before `/compact` runs | Last chance to save state that won't survive the summarisation. |
| `SessionEnd` | Session ends | Cleanup hook. Don't put critical work here — the user can `Ctrl-C` past it. |

A few plausible-sounding names that we've considered or seen
referenced but verified do *not* exist as hook events in 2026
Claude Code: `StopFailure`, `TeammateIdle`, `TaskCreated`,
`TaskCompleted`, `FileChanged`, `CwdChanged`, `ConfigChange`,
`WorktreeCreate`, `WorktreeRemove`, `MCP Elicitation`,
`ElicitationResult`, `InstructionsLoaded`, `PostCompact`. The
harness's settings.json loader won't fire anything if you register
under one of these keys; the entry is silently ignored. Worth
double-checking against the
[official changelog](https://code.claude.com/docs/en/changelog)
before adding a new hook entry to settings.

**One specific call-out** — `CronList` is sometimes mis-presented as
a hook event. It's a **CLI command / tool**, not a hook event. The
2.1.136 changelog entry that fixed `CronList` output "missing
qualifiers and the scheduled prompt" was about the command's display
output, not about a hook surface. You can't register
`hooks: { CronList: [...] }` in settings.json; the harness won't
fire anything.

## The effort knob (new in 2026)

Two related additions:

- The active effort level is exposed to hooks via the
  **`effort.level`** field in hook JSON input.
- The same level is exposed to *bash commands* via the
  **`$CLAUDE_EFFORT`** environment variable. Shipped in 2.1.133.

What this is for: effort is a session-wide knob that influences how
much the model "tries" — roughly, more inference time, more
deliberation, more verification — at higher cost. The hook input
exposure means **your enforcement layer can adapt to the user's
chosen effort.**

Concrete example: a `PreToolUse` hook that auto-approves safe `Bash`
commands at low effort (don't slow the user down for trivial things)
but routes everything to interactive permission at high effort (the
user explicitly said *try harder*, don't undermine that with
silent auto-approvals).

```sh
#!/usr/bin/env bash
# pre-tool-use.sh — auto-approve only at low effort
INPUT="$(cat)"
EFFORT_LEVEL=$(echo "$INPUT" | jq -r '.effort.level // "default"')
TOOL=$(echo "$INPUT" | jq -r '.tool_name')

if [ "$TOOL" = "Bash" ] && [ "$EFFORT_LEVEL" = "low" ]; then
  # safe-command allowlist check elided
  exit 0
fi
# default: let the harness apply normal permission rules
exit 0
```

You can also read `$CLAUDE_EFFORT` directly from a bash hook
without parsing JSON, which is the common pattern for short hooks.

## settings.json shape (what to copy)

The canonical shape lives at `~/.claude/settings.json` (user-level)
or `<project>/.claude/settings.json` (project-level):

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "/Users/me/.claude/hooks/pre-tool-use.sh"
          }
        ]
      }
    ],
    "PostToolUseFailure": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "/Users/me/.claude/hooks/log-failures.sh"
          }
        ]
      }
    ]
  }
}
```

The `matcher` field filters which specific tool/event variant the
hook fires for (e.g. `matcher: "Bash"` for only Bash tool calls).
Omitting the matcher means fire on all variants of the event.

Hook script contract:

- **stdin** receives a JSON blob describing the event. Schema varies
  by event but always includes `session_id`, `cwd`, `effort.level`,
  and event-specific fields (`tool_name` + `tool_input` for
  `PreToolUse`, `duration_ms` for `PostToolUse`, etc.).
- **stdout** is plumbed back into the Claude Code session as
  observation — useful for surfacing computed state to the model.
- **stderr** is shown to the user.
- **Exit codes**: `0` = continue silently. `2` = block + send
  stdout/stderr as feedback. Other non-zero = warning shown to
  user but doesn't block. **`Stop` hooks blocking 8 consecutive
  times stop blocking** (2.1.143 cap).

## terminalSequence (new in 2026)

The `terminalSequence` field in hook JSON *output* lets a hook emit
escape sequences for desktop notifications, terminal bell, and
window title updates — *without* the hook needing a controlling
terminal. Useful when you want a `Notification` hook to flash the
title without spawning a subprocess.

```json
// hook stdout
{
  "terminalSequence": "]0;Claude needs attention"
}
```

Combined with the `Notification` event, this is the cleanest way to
get desktop pings when Claude pauses for input on a long-running
session.

## Try this

1. Drop a `PostToolUseFailure` hook that logs every failure to
   `~/.claude/logs/tool-failures.jsonl`:

   ```sh
   #!/usr/bin/env bash
   # ~/.claude/hooks/log-failures.sh
   mkdir -p ~/.claude/logs
   # jq -c . forces single-line JSON regardless of whether stdin is
   # pretty-printed — otherwise multi-line input breaks JSONL semantics.
   jq -c . >> ~/.claude/logs/tool-failures.jsonl
   ```

   Register it in `~/.claude/settings.json` under the
   `PostToolUseFailure` event with no matcher. Run a Claude Code
   session that does something likely to fail (e.g. `Bash` to a
   missing command). Tail the log — observe the JSON, including the
   `duration_ms` and `effort.level` fields.

2. Add a `PreToolUse` hook with `matcher: "Bash"` that just echoes
   the command to stderr. Watch which Bash commands the model
   really wants to run as it works. This is a low-cost observability
   pattern.

3. *Don't* try to build self-healing retry loops with `Stop` hooks
   until you understand the 8-consecutive-block cap. The cap is
   load-bearing; without it, a hook that always blocks would soft-
   hang the agent.

## Sources

- [Anthropic Claude Code Changelog](https://code.claude.com/docs/en/changelog)
  — canonical event list and version-specific changes (2.1.119 for
  `duration_ms`, 2.1.133 for `effort.level`, 2.1.136 for the
  `CronList` output fix, 2.1.143 for the `Stop` 8-cap, 2.1.141 for
  `CLAUDE_CODE_PLUGIN_PREFER_HTTPS`).
- [Claude Code: Hooks, Subagents, and Skills — Complete Guide](https://ofox.ai/blog/claude-code-hooks-subagents-skills-complete-guide-2026/)
  — third-party 2026 reference covering all three extensibility layers.
- Anthropic Release Notes, May 2026 (Releasebot summary):
  <https://releasebot.io/updates/anthropic/claude-code>
- The April 2026 Changelog roll-up (versions 2.1.69 → 2.1.101),
  Apiyi.com:
  <https://help.apiyi.com/en/claude-code-changelog-2026-april-updates-en.html>
