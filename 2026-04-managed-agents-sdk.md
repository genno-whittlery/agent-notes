# 2026-04 — Managed Agents SDK

## Why this matters

In April 2026 Anthropic exposed a **Managed Agents API**, gated behind
the `managed-agents-2026-04-01` beta header. The headline shift: you
can now run multiple Claude Code-style agents through a single API
contract, with the platform managing isolation between them. This is
the building block under things like Anthropic's own Claude Cowork
demo, Cole Medin's 24-hour-coding-agent video, and the multi-agent
orchestration writeups that proliferated through April and May.

The reason it's a separate entry, not just an "SDK update" footnote:
the *isolation model* matters. All managed agents share a container
and a filesystem, but each runs in an isolated session thread with
its own configuration. That's a specific architectural choice — the
alternative would have been one-container-per-agent (heavier, fully
sandboxed) or shared-everything (lighter, but cross-contamination
risk). Anthropic picked the middle.

## The isolation model

| Layer | Shared across agents? | What that means |
|---|---|---|
| Container | **Yes** | All managed agents see the same filesystem. One agent's `git stash` is visible to another. |
| Network egress | Yes | Whatever the container can reach, every managed agent can reach. |
| Session thread | **No — isolated per agent** | Each agent has its own conversation, its own context window, its own tool call history. |
| Configuration | No | Each agent can have its own system prompt, allowed-tools list, model selection, hooks. |
| Long-term memory | No (today) | No managed cross-agent memory store as of the April beta; agents can persist via files (and thus share via the container filesystem). |

The "shared container, isolated session" model means **agents
collaborate through the filesystem**. One agent writes a file, another
reads it. The pattern is deliberately git-shaped: small,
human-inspectable artefacts as the inter-agent protocol, not opaque
RPC.

## What that buys you

Three patterns the community converged on through April and May:

### 1. Author + Reviewer split

Agent A writes a feature on a branch. Agent B reviews the same diff
with a different system prompt and a separate context window. Both
agents share filesystem visibility but neither sees the other's
conversation history. The review can be honest because the reviewer
*didn't see how the sausage was made*.

This is the pattern Code Review (May 2026 conference launch) builds
on under the hood.

### 2. Specialist dispatch

A main agent dispatches scoped tasks to specialist agents (test
writer, doc writer, migration runner). Each specialist runs with a
restricted allowed-tools list and a focused system prompt. The main
agent re-reads filesystem artefacts to know what happened, not the
specialist's full trace. Context window stays bounded.

### 3. Long-running watcher

Cole Medin's *24-hour coding agent* demo is this pattern at extreme:
one orchestrator agent dispatches scoped work to short-lived child
agents over a 24-hour window, with the orchestrator's job being to
re-read state and decide what to do next. Each child agent has a
narrow context; the orchestrator carries the long-horizon state.

## Token cost reality

The [Anthropic Managed Agents overview](https://platform.claude.com/docs/en/managed-agents/overview)
notes that subagent-heavy workflows can consume **around 7×** the
tokens of a single-thread session. The reason is straightforward when
you trace it: each agent dispatched sees its own system prompt
(large), its own initial context (often duplicated across siblings),
and produces its own output, and the orchestrator then re-reads
filesystem outputs and re-issues instructions.

The community guidance that emerged in 2026: **don't dispatch a
subagent for work that fits in your own context window**. The
isolation tax is real. Dispatch when:

- The subagent needs to read a lot of files just to produce a short
  answer (context-window-shaped wins).
- You need actual parallelism (multiple long-running tasks).
- You need a different system prompt or allowed-tools list than the
  parent.
- You want auditable independence (the reviewer-pattern argument).

Don't dispatch when:

- The task is small and you can do it in the current context window.
- The subagent will need to read the same files the parent already
  has loaded.
- You're using subagents because they "feel" parallel rather than
  because parallelism is structurally required.

## Beta header

In the SDK as of the April beta:

```python
# pseudocode — actual SDK exposes this without the user setting the
# header by hand
client.messages.create(
    ...
    extra_headers={"anthropic-beta": "managed-agents-2026-04-01"},
)
```

The SDK sets the header automatically. You only care about the header
name when:

- Reading API docs and matching feature-flag prose to your installed
  SDK version.
- Constructing raw HTTP requests outside the SDK.
- Diagnosing why a feature documented in a 2026 blog post doesn't
  work on your client — usually means an older SDK that doesn't set
  the beta header.

## Try this

If you have an Anthropic API key and the Python or TypeScript SDK
installed:

1. Read the Managed Agents overview page:
   <https://platform.claude.com/docs/en/managed-agents/overview>.
2. Implement the Author + Reviewer split on a toy repo. Two managed
   agents, same container. Agent A writes a function and commits.
   Agent B reviews the diff and posts a comment file. Compare what
   each agent saw in its conversation.
3. Now measure token use against a single-agent implementation of
   the same task. The 7× number is from Anthropic's own
   documentation — see if your toy gets close. (It usually does
   when the duplication of system prompts and initial file-reads
   is fully accounted for.)

The point isn't to "use managed agents." The point is to *feel* the
tradeoff before deciding when to use them.

## Sources

- *Multiagent sessions* — Claude API Docs:
  <https://platform.claude.com/docs/en/managed-agents/multi-agent>
- *Claude Managed Agents overview* — Claude API Docs:
  <https://platform.claude.com/docs/en/managed-agents/overview>
- Cole Medin, *Claude SDK: 24-Hour Coding Agent* (YouTube):
  <https://www.youtube.com/watch?v=BGouphNN5hg>
- Shipyard, *Multi-agent orchestration for Claude Code in 2026*:
  <https://shipyard.build/blog/claude-code-multi-agent/>
- Scopir, *Multi-Agent Orchestration for Developers in 2026:
  Running Claude, Codex, and Copilot in Parallel*:
  <https://scopir.com/posts/multi-agent-orchestration-parallel-coding-2026/>
- Cognition Labs, *claude-memory-compiler: the first serious
  Karpathy-wiki implementation for Claude Code*:
  <https://www.cognitionus.com/blog/claude-memory-compiler-guide>
- *Anthropic Release Notes — May 2026*, Releasebot summary:
  <https://releasebot.io/updates/anthropic>
