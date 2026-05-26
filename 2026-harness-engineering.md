# 2026 (rolling) — Harness engineering

## Why this matters

"Harness engineering" is the 2026 term-of-art for the practice of
*designing the layer that wraps the LLM* — not the prompt, not the
model, but everything around them: context assembly, tool surface,
memory persistence, control loop, output gating.
[Addy Osmani's piece](https://addyosmani.com/blog/agent-harness-engineering/)
defines it cleanly:

> "An AI harness is the operating layer around a language model
> that determines how context is assembled, which tools are
> available, how memory persists across turns, how the control
> loop runs, and which quality gates output must pass before
> reaching a user. What differentiates a reliable production agent
> from an unpredictable demo is almost always the harness design."

This is the conceptual umbrella under which most of the other
entries in this repo live. CLAUDE.md is a harness component
(standing-instruction context). Hooks are a harness component
(quality gates / deterministic control). Subagent dispatch is a
harness component (context isolation strategy). The Anthropic
source leak revealed that QueryEngine.ts *is the harness* in
implementation — 46K lines of context packing, retry logic,
streaming, cost accounting.

What changed in 2026: the term became load-bearing. Practitioner
accounts from Anthropic, OpenAI, and LangChain converged on
naming this layer. A
[Pinggy survey piece](https://pinggy.io/blog/best_ai_harnesses_to_supercharge_llm_models/)
cites the headline 2026 statistic:

> **"About 88% of AI agent projects never make it to production,
> mostly because the harness is too fragile."**

Whether the exact number is rigorous is unverifiable; the
direction is borne out across multiple reports. Production failure
modes are overwhelmingly *harness failures* (context overflow,
state loss, control-loop bugs), not *model failures* (model picked
the wrong answer). **Improving the harness on the same model
outperforms switching to a more capable model with a worse
harness.** This claim is repeated across every 2026 harness
writeup; it's the slogan of the whole topic.

## The patterns Anthropic shipped

Anthropic published
[`anthropics/cwc-long-running-agents`](https://github.com/anthropics/cwc-long-running-agents)
as a reference implementation of harness patterns for software
development that spans hours or days. Two patterns of note:

### Two-agent harness (initializer + coding)

[Rick Hightower's writeup](https://medium.com/@richardhightower/anthropics-harness-engineering-two-agents-one-feature-list-zero-context-overflow-7c26eb02c807)
characterises this pattern: an **initializer agent** runs once,
sets up structured environments — a feature list, a git repo,
progress-tracking files. A **coding agent** then runs
session-by-session, reading the structured state at start and
writing updated state at end. Each coding session is short and
context-bounded; the *cross-session state* lives on disk in
formats both agents can read.

Why it works: the LLM's context window is the bottleneck. Make the
LLM responsible only for one session's work; offload long-horizon
state to filesystem artefacts the next session reads. The
"agent that remembers" emerges from *state on disk*, not from a
model with longer memory.

### Three-agent harness (planning + generation + evaluation)

The
[InfoQ writeup](https://www.infoq.com/news/2026/04/anthropic-three-agent-harness-ai/)
covers Anthropic's three-agent harness for long-running full-stack
development:

- **Planning agent** — reads the feature list + current state,
  produces a plan for the next session
- **Generation agent** — executes the plan; writes code
- **Evaluation agent** — reads the generated code, runs tests,
  checks against the original requirements

Each agent runs in its own context. The agents communicate
through the filesystem (the same Anthropic-Managed-Agents
collaboration model from the
[entry above](./2026-04-managed-agents-sdk.md)). The evaluation
agent's output feeds back into the planning agent's next-session
input.

This is the *agentic version* of the spec → plan → code workflow
in the
[CLAUDE.md + plan mode entry](./2026-claude-md-and-plan-mode.md) —
the same three-stage discipline, but with each stage handled by an
isolated agent rather than by the same agent with human-review
gates between turns.

## Harness design patterns (the shared vocabulary)

Across multiple 2026 writeups, the recurring named patterns:

| Pattern | What it means |
|---|---|
| **Reason–act (ReAct)** | Interleave reasoning steps with tool calls; the agent's thought trace is part of its conversation. |
| **Retrieval** | Just-in-time context loading from external stores (file paths, vector embeddings, web search) rather than pre-loading everything. |
| **Reflection** | The agent's own output gets re-examined by the agent (or by a paired agent) before being accepted. |
| **Verification** | External checks (tests pass, lint clean, build succeeds) gate the output before it counts as done. |
| **Memory** | Persistent state across sessions, usually filesystem-backed. |
| **Search** | Dynamic context expansion via tool calls (`Grep`, `Glob`, Web search) rather than pre-determined context. |
| **Orchestration** | Multi-agent coordination — dispatch, isolation, reconciliation. |

A serious harness combines several of these. Claude Code's
default harness already uses reason–act (the model's tool-use
loop), retrieval (file-reading on demand), verification (the
permission system gates writes), and search (`Grep` / `Glob`).
You add memory + reflection + orchestration as you add
CLAUDE.md + subagents + plan mode on top.

## Why this is *the* lens

A useful shift in how to think about Claude Code's design choices:
**every weird-feeling decision usually traces back to a harness
constraint, not a model constraint.**

Examples:

- *Why does CC ask me before every Bash command?* Verification
  pattern; the harness gates output before it commits to disk.
- *Why does plan mode dispatch a research sub-agent?* Retrieval
  pattern + orchestration; gather context in an isolated agent so
  the main agent's context stays bounded.
- *Why is there an 8-block cap on `Stop` hooks?* Control-loop
  safety; the harness must prevent infinite loops *even when
  extension code misbehaves*.
- *Why does CLAUDE.md load once per session?* Memory pattern;
  cross-session state is expensive to re-process, so it gets one
  efficient load.
- *Why does compaction run automatically?* Memory + retrieval;
  the harness has finite working memory and has to recycle it
  productively.

None of these are about what the LLM can or can't do. They're all
harness choices. Once you have the pattern names, the design
decisions read as deliberate craft instead of arbitrary product
behaviour.

## The implication for your own tooling

If you're building anything Claude-Code-adjacent — a plugin, an
agent wrapper, an internal team workflow — the 2026 consensus is:

1. **The harness is where the work is.** Spending time on prompts
   is usually misallocated effort relative to spending time on the
   harness that surrounds them.
2. **Filesystem is the cheapest cross-context store.** Don't
   reach for a database or vector store before you've tried "have
   the agent read and write structured files."
3. **Verification is non-optional.** A pattern that doesn't end
   in something checkable (tests pass, lint clean, types check)
   is a pattern that will silently fail in production.
4. **One agent per concern.** If you find yourself loading the
   same agent with planning + generation + evaluation
   responsibilities, you're inside the failure mode of the 88%
   that don't ship. Split.

## Try this

The Anthropic
[`cwc-long-running-agents`](https://github.com/anthropics/cwc-long-running-agents)
repo is the most concrete starting point. Clone it, read the
README, run the worked examples. Even if you don't end up using
the harness directly, reading it teaches you how Anthropic *expects*
production harnesses to be built — the patterns are visible in the
code.

After that, the
[*Natural-Language Agent Harnesses*](https://arxiv.org/html/2603.25723v1)
arxiv paper (March 2026) is the academic version of the same
material — denser but more rigorous about the claims.

## Sources

- *Agent Harness Engineering* — Addy Osmani:
  <https://addyosmani.com/blog/agent-harness-engineering/>
- *AI Harness Engineering: The Layer That Makes Your LLM
  Applications Actually Work* — Pinggy blog (carries the 88% stat):
  <https://pinggy.io/blog/best_ai_harnesses_to_supercharge_llm_models/>
- *Anthropic's Harness Engineering: Two Agents, One Feature List,
  Zero Context Overflow* — Rick Hightower, Medium (March 2026):
  <https://medium.com/@richardhightower/anthropics-harness-engineering-two-agents-one-feature-list-zero-context-overflow-7c26eb02c807>
- *Anthropic Designs Three-Agent Harness Supports Long-Running
  Full-Stack AI Development* — InfoQ (April 2026):
  <https://www.infoq.com/news/2026/04/anthropic-three-agent-harness-ai/>
- *Anthropic: Long-Running Agent Harness for Multi-Context
  Software Development* — ZenML LLMOps Database:
  <https://www.zenml.io/llmops-database/long-running-agent-harness-for-multi-context-software-development>
- [`anthropics/cwc-long-running-agents`](https://github.com/anthropics/cwc-long-running-agents)
  — Anthropic's reference implementation
- *Natural-Language Agent Harnesses* — arxiv:
  <https://arxiv.org/html/2603.25723v1>
