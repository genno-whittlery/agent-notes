# 2026-04 — Hermes Agent (Nous Research) + ACP protocol

## Why this matters

Hermes Agent is **Nous Research's open-source CLI coding agent**.
It's worth a separate entry in a Claude-Code-focused notes set
because it isn't a Claude Code alternative — it's a coding agent
that *delegates to Claude Code* (and to Codex CLI, OpenCode, etc.)
while keeping ownership of session lifecycle, task tracking, and
cross-session memory. In April 2026 it shipped **generalized ACP
(Agent Client Protocol) support**, which is the cleaner shape that
the wider multi-agent ecosystem is converging on.

The interesting thing for a Claude Code user: Hermes is the most
visible 2026 example of "an orchestrator that runs *above* Claude
Code." If managed agents (entry above) are the in-Anthropic way to
do multi-agent, Hermes is the out-of-Anthropic way — and it works
with the public CLI surfaces (`claude --print`, `claude` interactive)
rather than the Managed Agents API.

## What Hermes is

A single Rust-built CLI binary from
[NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent).
Two halves matter:

- **The agent loop** — Hermes runs its own reasoning loop, with its
  own context window, its own memory store, and its own skill
  format (a `SKILL.md` shape compatible with the same agentskills.io
  open standard Claude Code uses).
- **The delegation layer** — Hermes can hand work off to underlying
  coding CLIs. The headline two: Claude Code and Codex CLI. The
  Webwright integration list adds Pi and OpenCode.

What makes Hermes interesting (vs. just "another Cursor-shaped
tool") is the **persistent-state-across-sessions** posture: Hermes
keeps a structured store of project layout, conventions, and
in-flight work. Restart the terminal, restart Hermes, it
remembers. The pitch is "the agent that learns the longer you run
it" — explicitly framed against the per-session-amnesia default
of most coding-agent tools.

## Two modes of Claude Code delegation

Per the
[Hermes docs on Claude Code integration](https://hermes-agent.nousresearch.com/docs/user-guide/skills/bundled/autonomous-ai-agents/autonomous-ai-agents-claude-code):

### Print mode (one-shot)

```
hermes> delegate to claude: "refactor src/parser.ts to use the new AST helpers"
```

Hermes wraps this as `claude --print "<prompt>"`, runs CC headless,
captures the result, and returns to its own loop. This is the
cleaner integration path — clear input, clear output, no
interactive state to reconcile.

### Interactive mode (REPL)

```
hermes> open claude interactive in this workspace
```

Hermes spawns CC interactively, hands the terminal over, and waits.
You drive CC as you normally would (slash commands, multi-turn
conversation), and on CC exit Hermes resumes ownership of its
session — reading the resulting filesystem state into its memory
store.

Print mode is what you use for *delegated tasks*. Interactive mode
is what you use when *you want to drive CC directly inside a
Hermes-managed session* — Hermes stays out of the loop until you
exit CC.

## ACP — the April 2026 generalization

Originally Hermes hard-coded its CC integration. As of April 2026,
per
[NousResearch/hermes-agent issue #5257](https://github.com/NousResearch/hermes-agent/issues/5257),
Hermes is implementing **generalized ACP (Agent Client Protocol)
support**. ACP standardises agent-to-agent communication over
**stdio + NDJSON** — the same wire format Claude Code's own hooks
use, by no coincidence.

Why this matters: ACP makes Hermes's delegation surface
*pluggable*. The same protocol shape works for delegating to
Claude Code, Codex CLI, Gemini CLI, OpenCode, Pi, or any future
agent that speaks ACP. Hermes becomes a *protocol* user, not a
hard-coded integration set.

This is the same architectural pattern as
[Herdr's agent multiplexer](https://herdr.dev/) (newline-delimited
JSON socket API) — and as the Skills open standard
(agentskills.io's cross-vendor `SKILL.md` format). 2026's
quietly-important story: the multi-agent ecosystem is converging
on **stdio + NDJSON** as the inter-process contract.

## Why Nous Research specifically

Worth a one-paragraph framing because Nous's reputation precedes
the agent: Nous Research is best known in 2024–2025 for the
**Hermes finetune series** (Hermes 1/2/3 — Llama-based open-weight
models that consistently scored well on coding and reasoning
benchmarks). The Hermes *Agent* inherits the same name and the
same open-source-first ethos: model-provider-agnostic, gives users
the option to bring their own backend.

[Setting up Hermes through Haimaker](https://haimaker.ai/blog/hermes-vs-codex/)
exposes the model-flexibility argument directly — 300+ models
behind one API key, switch between Claude, GPT, Gemini, and
open-weight models depending on the task. This is the *opposite*
posture from managed agents (which assume Anthropic backend), and
the *complementary* posture to Claude Code itself (which is
Anthropic-only by design).

Where it fits in your stack:

| If you want… | Use |
|---|---|
| The fastest path to a CC session with Anthropic models | Claude Code directly |
| Multi-instance orchestration with Anthropic-only models | Managed Agents (entry above) |
| Multi-agent orchestration across vendors (Claude + GPT + Gemini + open weights) | Hermes Agent |
| Terminal-level multiplexing of agent sessions | [Herdr](https://herdr.dev/) (entry not yet written; complementary to Hermes) |

These aren't competitors stacked against each other. They're
different layers — Hermes operates above the CLIs; Herdr
operates above the terminal. You can run a Hermes session inside a
Herdr workspace inside a tmux pane, delegating to Claude Code
inside Managed Agents. Realistically no one runs all four together,
but the stack composes.

## Try this

If you want to feel the Hermes-as-orchestrator-over-CC posture
without committing to it as your daily driver:

1. Install Hermes from the
   [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)
   GitHub releases. Pre-built binary for macOS / Linux.
2. Run it in a project directory. Watch the structured-state
   initialization — Hermes creates its own bookkeeping files
   (similar in spirit to what Anthropic's cwc-long-running-agents
   harness does; see the
   [harness engineering entry](./2026-harness-engineering.md)).
3. Try a single delegation: `delegate to claude: "explain the
   architecture of this codebase in 3 paragraphs"`. Watch Hermes
   build the `claude --print` invocation, capture the output, and
   re-incorporate it into its own session memory.
4. Quit Hermes. Restart it. Run `recall` (or whatever the docs say
   the session-recall command is at the time you read this — names
   change). Notice it remembers what you were working on. That's
   the load-bearing UX difference vs. per-session-amnesia tools.

If you do this regularly enough that you're considering switching
to Hermes as a daily driver: the
[haimaker comparison piece](https://haimaker.ai/blog/hermes-vs-codex/)
is the honest "when does this actually beat just using CC directly"
analysis. Short answer: when model flexibility matters more than
the Anthropic-specific affordances (latest model first, deep
ecosystem of Anthropic-only plugins).

## Sources

- [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)
  — the canonical repo, binary releases, issue tracker
- [Hermes Agent docs — Claude Code integration page](https://hermes-agent.nousresearch.com/docs/user-guide/skills/bundled/autonomous-ai-agents/autonomous-ai-agents-claude-code)
- [Hermes Agent docs — AI providers](https://hermes-agent.nousresearch.com/docs/integrations/providers)
- [Hermes Agent — Skills Hub](https://hermes-agent.nousresearch.com/docs/skills)
  (compatible with agentskills.io open standard)
- *Hermes Agent vs Codex CLI: Which Coding Agent to Use (2026)* —
  Haimaker blog: <https://haimaker.ai/blog/hermes-vs-codex/>
- [Issue #5257 — Generalized ACP client for multi-agent CLI
  orchestration](https://github.com/NousResearch/hermes-agent/issues/5257)
  — the April 2026 ACP work
- [Warp issue #10430 — adding Hermes Agent as a third-party CLI
  agent](https://github.com/warpdotdev/warp/issues/10430) — useful
  signal that Hermes is getting first-class integration in
  developer terminals
