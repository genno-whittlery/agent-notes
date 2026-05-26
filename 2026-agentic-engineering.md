# 2026 (rolling) — Agentic engineering: the umbrella

## Why this is its own entry

"Agentic engineering" is the **umbrella discipline** under which
nearly every other entry in this repo lives. Distinct from
"prompt engineering" (which 2026 has retired as the load-bearing
term — see the [CLAUDE.md + plan mode entry](./2026-claude-md-and-plan-mode.md))
and broader than "harness engineering" (which is one *part* of
agentic engineering), it names the *practice* of designing,
building, and operating AI agents as production systems.

The term crystallised across 2026 alongside the conference
circuit (see the [AI Engineer Europe entry](./2026-04-aie-europe.md)),
and the [`agentic-engineering` open-standard skills repo](https://github.com/netresearch/claude-code-marketplace)
positions itself explicitly under this banner. Senior
practitioners use it as the umbrella category their work falls
under; recruiters use it to describe the role; conference tracks
organise around it.

## What it actually covers

A working definition: **agentic engineering is the practice of
engineering reliable behaviour from non-deterministic AI agents.**
The core skills cluster into seven sub-disciplines, every one of
which has at least one dedicated entry in this notes set:

| Sub-discipline | Covered in |
|---|---|
| **Harness engineering** — the layer around the LLM (context, tools, memory, control loop, gates) | [harness engineering entry](./2026-harness-engineering.md) |
| **Context engineering** — what goes into the model's context window, when, and how it's recycled | [CLAUDE.md + plan mode entry](./2026-claude-md-and-plan-mode.md) — references the Anthropic *Effective context engineering* post |
| **Skill engineering** — packaging reusable instructions / capabilities for cross-context activation | [skills + marketplace entry](./2026-skills-and-marketplace.md) |
| **Hook / control-flow engineering** — deterministic enforcement of guarantees the LLM alone can't provide | [hooks + effort entry](./2026-hooks-and-effort.md) |
| **Multi-agent orchestration** — when to dispatch, isolate, reconcile across agents | [managed agents entry](./2026-04-managed-agents-sdk.md), [Hermes Agent entry](./2026-04-hermes-agent.md) |
| **Spec-driven workflow design** — three-document (spec → plan → code) discipline with human review between each | [CLAUDE.md + plan mode entry](./2026-claude-md-and-plan-mode.md) |
| **Evaluation and feedback** — how you tell whether the agent is doing the right thing without re-reading every output | covered in passing across the entries; no dedicated entry yet |

This taxonomy isn't authoritative — different practitioners
group differently. But the *shape* of "there are sub-skills, and
they cluster" is converged: the Latent Space crowd, the Anthropic
engineering blog, the Cognition Labs writeups, and the
ThursdAI / AIE Europe speakers all use roughly these categories.

## Why this matters for the rest of the repo

Two practical implications:

### 1. Skills compound

If you're spending time on Claude Code, you're not just learning
"a tool." You're acquiring the *agentic engineering skill set*,
which transfers across tools. The CLAUDE.md discipline ports to
Cursor rules; the spec-driven workflow ports to Codex CLI; the
hook patterns port to any tool that exposes a settings.json-like
extension surface. The harness-engineering principles port to
Hermes Agent, OpenCode, anything ACP-compatible (see the
[Hermes entry](./2026-04-hermes-agent.md)).

This is consistent with Tom Blomfield's economics framing (see
the [Blomfield entry](./2026-03-blomfield-economics.md)): the
leverage you get from agentic-engineering skill *compounds across
tools*, which is why the per-user payoff is larger than what
previous dev-productivity skills returned.

### 2. The discipline is what survives model churn

Models change every few months in 2026. The specific
Anthropic / OpenAI / DeepSeek release isn't durable. The
*discipline* of agentic engineering is. A CLAUDE.md you write
today will work against GPT-5, Gemini 3, or the next DeepSeek
release — because the skill of *writing standing instructions
for a non-deterministic agent* doesn't care which agent. The
spec-driven workflow doesn't depend on which CLI you're using.
The hook-pattern discipline doesn't care which harness exposes
the hooks.

This is why the notes-set is dated by month: implementations
churn; the discipline doesn't. Six months from now, the
specific Anthropic-changelog entries will be different. The
practice of structured context loading, spec-before-code,
hook-based enforcement, and isolated subagent dispatch will be
recognisable.

## The career-shape signal

A worth-flagging meta-observation from the AIE Europe debrief +
the Blomfield observations + Anthropic's own internal data:

**"Agentic engineer" is solidifying as a job title in 2026.**
Job listings, recruiter outreach, conference speaker bios all
use the term. The shape: someone who treats the LLM as a
component to be engineered around, not a service to be prompted.

This isn't a new role — senior software engineers using AI
agents for production work were doing this in 2024 — but the
*name* matters for hiring discoverability, for compensation
benchmarking, and for telling a coherent story about what
skill set you're investing in.

## What we're not covering (yet)

Evaluation infrastructure is the obvious gap in this repo right
now. The agentic-engineering discipline has a fast-moving
sub-field around *how do you tell whether your agent is doing
the right thing* (LLM-as-judge, structured evals, behavioural
red-teaming, regression tests across model versions). This
deserves its own dedicated entry once the patterns stabilise.

## Try this

The exercise that makes the taxonomy concrete: pick a single
problem you've solved with Claude Code in the last week. Walk
through each of the seven sub-disciplines above and ask
**"how did this manifest in that work?"**

- *What harness shape did I rely on?* (Often: CC's defaults.)
- *What context did I curate?* (CLAUDE.md? `@`-references?
  Just the open files?)
- *Did I use any skills, or could I have?*
- *Did I hook anything?* (Probably not, but should you have?)
- *Did I dispatch subagents?* (Manually via Task tool? Or did
  CC do it via plan mode automatically?)
- *What spec or plan preceded the code?* (Inline in chat? A
  plan.md? Or just "do the thing"?)
- *How did I verify it worked?*

The exercise isn't "score yourself." It's *make the implicit
explicit*. The discipline is doing the seven things deliberately;
the failure modes are doing them implicitly and getting random
results.

## Sources

- *Scaling without Slop* — Swyx, Latent Space (2026 framing
  essay): <https://www.latent.space/p/2026>
- *Effective context engineering for AI agents* — Anthropic
  Engineering blog:
  <https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents>
- *Agent Harness Engineering* — Addy Osmani:
  <https://addyosmani.com/blog/agent-harness-engineering/>
- *AIE Europe Debrief + Agent Labs Thesis* — Latent Space
  (post-AIE-Europe synthesis):
  <https://www.latent.space/p/unsupervised-learning-2026>
- [`netresearch/claude-code-marketplace`](https://github.com/netresearch/claude-code-marketplace)
  — community-curated agentic-engineering skill collection,
  positioned explicitly under the umbrella
- Cross-references: every other entry in this repo falls under
  one or more of the seven sub-disciplines above
