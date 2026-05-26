# 2026-03 — The source leak and what 512K lines reveal

## Why this matters

On 2026-03-31, Anthropic accidentally published the *entire*
unobfuscated Claude Code source — about **512,000 lines of TypeScript
across roughly 1,900 files**
([Layer5 engineering analysis](https://layer5.io/blog/engineering/the-claude-code-source-leak-512000-lines-a-missing-npmignore-and-the-fastest-growing-repo-in-github-history/))
— to npm. Cause: a missing `.npmignore` that should have excluded
internal source from the published tarball. The package was pulled
within hours, but the cat was out of the bag.

Within a week, an academic paper landed on arxiv —
*Dive into Claude Code: The Design Space of Today's and Future AI
Agent Systems* (VILA-Lab, [arXiv:2604.14228](https://arxiv.org/html/2604.14228v1))
— and multiple security-and-architecture writeups dissected the leak.
For the first time, the harness was a thing you could read end-to-end.

The leak isn't important because Claude Code is now somehow "open
source" — it's not, and Anthropic was clear about that. It's
important because **the architecture stopped being a black box for
the people building on top of it**. The patterns the community had
been reverse-engineering became things you could just go read.

## The headline file: `QueryEngine.ts`

The biggest single module in the leaked source is **`QueryEngine.ts`,
roughly 46,000 lines**
([Layer5 analysis](https://layer5.io/blog/engineering/the-claude-code-source-leak-512000-lines-a-missing-npmignore-and-the-fastest-growing-repo-in-github-history/),
[VILA-Lab arxiv §3](https://arxiv.org/html/2604.14228v1)). It is the
harness. Specifically, it handles:

- Prompt construction (system prompt assembly, CLAUDE.md hierarchy
  expansion, tool definition injection, context window packing)
- Streaming response handling (token-by-token streaming, tool-call
  interception)
- Token counting (live tracking against the model's limit, the basis
  for `/compact` triggering)
- Cost tracking (USD per session, the number `claude status` reports)
- Retry logic (exponential backoff, idempotency, "the API call that
  failed twice but the third one worked" UX)

The takeaway for someone writing extensions: **almost every
extension point you care about ultimately calls back into
QueryEngine.** Hooks intercept events QueryEngine emits. Skills get
injected into the prompts QueryEngine constructs. Subagent dispatch
goes through QueryEngine. The platform-shaped story is true at the
code level: this is one harness with many faces.

## The five values the architecture is motivated by

The VILA-Lab paper extracts five values that the architecture is
designed around. These aren't marketing — they're values you can
trace to specific implementation choices in the leak:

| Value | Concrete implementation evidence |
|---|---|
| **Human decision authority** | The permission system. Tool calls that touch a write surface or shell trigger an interactive prompt by default. The exit-code-2 "block" semantic in hooks. CLAUDE.md as a place humans declare standing decisions. |
| **Safety and security** | The sandbox layer. The `--dangerously-skip-permissions` flag's deliberately scary name. The permission-prompt requirement on `Bash`. MCP server trust gating. |
| **Reliable execution** | Retry logic in QueryEngine. The streaming + checkpointing pattern. The `Stop` hook 8-consecutive-block cap *(supersede note added 2026-05 after the cap shipped in 2.1.143)* — *prevent infinite loops even when extension code misbehaves*. |
| **Capability amplification** | Tool composition (Read+Edit+Bash+WebSearch as a small set of orthogonal primitives that compose into anything). Subagent dispatch. Skills as reusable capability extensions. |
| **Contextual adaptability** | CLAUDE.md hierarchy. `--add-dir`. Per-project + user + local settings layering. The way `cwd` propagates through hook input. |

What's useful about this list isn't that you'll memorise it. It's
that when a Claude Code design decision feels weird, you can usually
trace it to one of these five values. The hooks-block-at-exit-2
semantic feels overly restrictive until you read it as "Human
decision authority wins; the human controls the loop." The
permission prompt on every shell command feels annoying until you
read it as "Safety wins by default; you opt out per session."

## What the leak revealed about context engineering

The single thing in the leak with the most impact on how people use
Claude Code is the **context-window packing logic in QueryEngine**.
Reading the actual code (or the VILA-Lab paper's summary of it) made
several things concrete that the community had been guessing:

- **CLAUDE.md is loaded once per session, near the top of the system
  prompt.** It is *not* re-read on every turn. Editing CLAUDE.md
  mid-session has no effect until the next session start (or a
  `/init` triggered re-load).
- **`@`-references load file contents *into the conversation*, not
  into the system prompt.** They cost user-message tokens, not
  system-prompt tokens. Important for `/compact` semantics.
- **Tool definitions** (the JSON schemas the model sees for each
  available tool) are non-trivial in token cost. Plugins that bundle
  large tool surfaces have a real per-session tax that
  `claude plugin details` now surfaces.
- **The auto-`/compact` trigger** has a specific threshold (visible
  in the leak; documented after) rather than the "around 70%" folk
  wisdom — though 70% is the right number to use as a heuristic
  because the actual threshold varies with model and the trigger
  fires *before* a hard wall.

These four points reshaped the community's "how do I keep CC
performant on long sessions" advice in April and May 2026. The folk
wisdom moved from "use /compact when it feels full" to "use
/compact proactively before the trigger fires, treat tool surface
as a cost, and treat CLAUDE.md as one-shot per session."

## What Anthropic did about the leak

Within 24 hours: pulled the package, rotated relevant secrets where
the leak exposed any, and acknowledged the incident publicly. The
incident *did not* change Anthropic's posture on whether Claude Code
will be open-sourced — it remains a closed-source product. The
detailed harness analysis the leak enabled has become part of how
the ecosystem thinks about extending Claude Code; post-leak
Anthropic documentation (notably the
[*Effective context engineering*](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
engineering post) includes implementation-level specifics about how
context packing works that older Anthropic documentation did not.

## Try this

You can no longer read the leaked source directly — Anthropic
removed it and reasonable people respect that. But:

1. Read the [VILA-Lab arxiv paper](https://arxiv.org/html/2604.14228v1).
   It's the most thorough public analysis of the leak's findings,
   with section-level breakdowns of the major modules.
2. Cross-reference against Anthropic's own [post-leak engineering
   blog posts](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
   which surface implementation specifics in their own words.
3. Watch your own Claude Code session's `claude status` output as
   you work. The cost-tracking and context-percentage numbers are
   QueryEngine's bookkeeping made visible. The mental model behind
   those numbers is what the leak made legible.

## Sources

- *Dive into Claude Code: The Design Space of Today's and Future
  AI Agent Systems* — VILA-Lab,
  [arXiv:2604.14228](https://arxiv.org/html/2604.14228v1)
- *The Claude Code Leak of 2026* — Hareem Fatima, Data & Beyond
  (Medium, April 2026):
  <https://medium.com/data-and-beyond/the-claude-code-leak-of-2026-anthropic-accidentally-gave-the-world-its-most-detailed-ai-db5adcebbe69>
- *The Claude Code Source Leak: 512,000 Lines* — Layer5 engineering
  blog:
  <https://layer5.io/blog/engineering/the-claude-code-source-leak-512000-lines-a-missing-npmignore-and-the-fastest-growing-repo-in-github-history/>
- *Claude Code Architecture Analysis* — bits-bytes-nn (2026-03-31):
  <https://bits-bytes-nn.github.io/insights/agentic-ai/2026/03/31/claude-code-architecture-analysis.html>
- *Effective context engineering for AI agents* — Anthropic
  Engineering blog (post-leak, draws on the same internals):
  <https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents>
