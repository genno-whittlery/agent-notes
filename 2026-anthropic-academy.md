# 2026 (rolling) — Anthropic Academy

## Why this matters

[**anthropic.com/learn**](https://www.anthropic.com/learn) is
Anthropic's own structured-learning platform — branded *Anthropic
Academy* internally — covering courses, guides, and certificates
across the three "Claude paths" the company is targeting:

| Path | Audience |
|---|---|
| **Build with Claude** | Developers building Claude-powered applications |
| **Claude for work** | Organisations implementing Claude across teams |
| **Claude for personal** | Individual users |

Featured courses listed at the time of writing: *Claude 101*,
*Claude Code in action*. Guides cover API development, the Model
Context Protocol (MCP), and Claude Code specifically. Certificates
are awarded on completion.

For a notes set focused on agentic engineering, Anthropic Academy
matters as **the first-party canonical pedagogy** — what
*Anthropic itself* says you should know, in the order they say
you should learn it. Worth knowing about for two reasons.

## Why first-party pedagogy is load-bearing

The 2026 ecosystem has a lot of community-authored learning
material (third-party tutorials, Medium posts, YouTube channels,
the writeups cited across this repo). Quality varies; accuracy
varies more. The
[hooks-and-effort entry](./2026-hooks-and-effort.md) call-out of
"13 plausible-sounding hook event names that don't actually
exist" is the canonical example of what happens when LLM-generated
tutorials don't get verified against first-party docs.

Anthropic Academy is the *first-party reset point*. When community
material drifts (event names, API shapes, version-specific
behaviour), the Academy is the version Anthropic stands behind.
For a serious learner, the workflow is:

1. **Read the Academy material** for the canonical first pass on
   any topic.
2. **Cross-reference the [changelog](https://code.claude.com/docs/en/changelog)**
   for what changed since the Academy course was written.
3. **Then read community material** for tacit knowledge, edge
   cases, and the "what does this look like in production"
   colour the official material rarely provides.

Doing it in the other order — start with community, only
fact-check later — is how community-error patterns propagate.
The 13-name "events that don't exist" callout in the hooks entry
is the case study.

## Where it fits in this notes set

Reading this entry in particular, the implicit Academy-mapping
across the repo:

| Academy course / guide | This repo's parallel entries |
|---|---|
| Claude 101 | (not covered — this repo skips from-zero intros) |
| Claude Code in action | [CLAUDE.md + plan mode](./2026-claude-md-and-plan-mode.md), [hooks](./2026-hooks-and-effort.md), [skills](./2026-skills-and-marketplace.md), [subagents/managed-agents](./2026-04-managed-agents-sdk.md) |
| MCP guide | [skills + marketplace](./2026-skills-and-marketplace.md) (where MCP fits in the plugin layout) |
| API guides | Out of scope; this repo focuses on the CLI / harness / agent layers, not raw API |

The Academy is broader (covers raw API usage); this notes set is
narrower (focuses on the agent/harness/CLI surfaces). They
overlap on the Claude Code material but don't replace each other.

## What's notably *not* on Anthropic Academy

A useful framing: the Academy covers what Anthropic *officially
supports*. What it *doesn't* cover is where the action is for an
agentic-engineering practitioner:

- **Cross-vendor patterns.** The Academy is Anthropic-shaped;
  agentskills.io, the ACP protocol (see the
  [Hermes Agent entry](./2026-04-hermes-agent.md)), and local-
  inference backends (see the [ds4.c entry](./2026-05-ds4-c-local-inference.md))
  aren't in the Academy curriculum even though they're
  load-bearing in 2026 practice.
- **Internal-Anthropic patterns from the leak.** The
  [source-leak entry](./2026-03-source-leak-architecture.md)
  surfaces architectural details that aren't in any Academy
  course because Anthropic doesn't (and probably shouldn't) build
  curriculum around leaked internals.
- **Economics-of-agentic-coding framing.** Tom Blomfield's
  observations (see [Blomfield entry](./2026-03-blomfield-economics.md))
  aren't pedagogy Anthropic would publish themselves — it's
  *external* commentary on what their tools enable.
- **Harness engineering as an organising concept.** The Academy
  teaches the *pieces* (hooks, skills, agents) but not the
  *umbrella* — that crystallised in the
  [harness engineering entry](./2026-harness-engineering.md)
  via community + arxiv work, not Anthropic curriculum.

This isn't a complaint about the Academy — first-party pedagogy
appropriately stays close to first-party surfaces. It's a
*scope-completeness check*: the Academy is necessary but not
sufficient for agentic-engineering literacy in 2026.

## Try this

If you're new enough to Claude Code that any of the entries in
this repo confused you on first read:

1. **Spend an hour on *Claude 101* and *Claude Code in action*
   on the Academy.** They'll close the gap on prerequisites this
   repo assumes.
2. **Come back and re-read whatever entry confused you.** It
   should land better with the Academy's vocabulary in place.

If you're already running Claude Code daily and find the Academy
courses "below your level":

1. **Do them anyway, on 2x speed if necessary.** What you're
   looking for isn't the new-to-you material; it's *the gaps
   between what Anthropic teaches and what you've picked up
   from community sources*. Those gaps are where the community-
   error patterns hide.
2. **Pair the Academy courses with the changelog page open.**
   The courses are static; the platform moves. Reconciling the
   two is the actual learning.

## Sources

- [Anthropic Academy](https://www.anthropic.com/learn) — the
  canonical landing page; structure may evolve over time
- [Anthropic Claude Code changelog](https://code.claude.com/docs/en/changelog)
  — the changelog is the always-current companion to the
  Academy's snapshot-style course material
- Cross-references: every entry in this repo is in some sense an
  *extension* of what the Academy covers — community-authored
  notes on community-cited patterns. The Academy is the
  baseline; the rest of this repo is the delta
