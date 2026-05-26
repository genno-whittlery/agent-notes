# Who we're following — intake list for Claude Code, 2026

The people, podcasts, aggregators, and adjacent-ecosystem voices we
read regularly to keep the rest of the repo current. Entries marked
**★** are already cited in one of the six entries. Entries without
are on the watch-list but haven't shown up as load-bearing yet.

Same shape as the entries themselves: dated when possible, honest
about what each source covers and doesn't, no implicit ranking.

## Anthropic-first-party

| Source | What | Where | Cadence |
|---|---|---|---|
| **★ Anthropic Engineering blog** | Implementation-level posts (context engineering, agent architecture, safety) | <https://www.anthropic.com/engineering> | Irregular — averages ~1–2 deep posts/month |
| **★ Anthropic Claude Code changelog** | Per-version notes for the CLI; canonical for hook events + flag changes | <https://code.claude.com/docs/en/changelog> | Per release (≈ every 1–2 days during 2026) |
| **Anthropic News / Product** | Launches + feature framing (Code Review, Managed Agents, Code w/ Claude) | <https://www.anthropic.com/news> | Per launch |
| **★ Claude Cookbook** | Worked examples for SDK + agent patterns | <https://platform.claude.com/cookbook/> | Irregular; grows during big launches |
| **★ Claude Code docs** | The reference for CLI / config / settings.json | <https://code.claude.com/docs/en/> | Continuous |
| **★ Boris Cherny** (Head of Claude Code) | Direction-of-travel framing; "coding is solved, what comes next" | Podcast circuit; X/Twitter | When invited on podcasts |
| **★ Cat Wu** (founding engineer, Claude Code) | Engineering-side framing; tools & UX choices | Podcast circuit | Less frequent than Cherny |

## Independent voices (already in our citations)

| Source | What | Where |
|---|---|---|
| **★ Simon Willison** | Live-blogs Anthropic events, runs cited deep dives on CC behaviour | <https://simonwillison.net/> |
| **★ Cole Medin** | YouTube long-form — agent SDK demos, 24-hour-agent style experiments | <https://www.youtube.com/@ColeMedin> |
| **★ Cognition Labs** | Devin-team writing on agent memory, Karpathy-wiki patterns | <https://www.cognitionus.com/blog> |
| **★ Forrest Chang** | The 220K+ ⭐ CLAUDE.md fork-network anchor | <https://github.com/ForrestChang> (fork-network entry point) |
| **★ Scott Spence** | Practical write-ups on plugin marketplaces + skill organisation | <https://scottspence.com/> |
| **★ Mark Chen** | Medium walkthroughs (skills marketplace, beginner-friendly) | <https://medium.com/@markchen69> |
| **★ Wataru Takahashi** | Spec-driven development series on Substack/Medium | <https://watarutakahashi.substack.com/> |
| **★ VILA-Lab** | Academic analysis of the source leak; arxiv 2604.14228 + follow-ups | <https://github.com/VILA-Lab/Dive-into-Claude-Code> |

## Podcasts (audio-format CC content)

| Show | Hosts | Why listen |
|---|---|---|
| **★ Latent Space** | Swyx + Alessio | The cross-vendor agent-tooling conversation; gets the operators-of-AI-engineering people. Latentspace.ai |
| **★ The Pragmatic Engineer** | Gergely Orosz | Engineering-management framing; Boris Cherny appearance (March 2026) is the most cited single CC interview |
| **★ Every / AI & I** | Dan Shipper | Lighter framing, but the Cherny + Wu joint appearance (October 2025) was foundational for "how we use Claude Code inside Anthropic" |
| **★ Lenny's Newsletter Podcast** | Lenny Rachitsky | Product-management lens; Cherny "what comes after coding is solved" (Feb 2026) |
| **No Priors** | Sarah Guo + Elad Gil | Less CC-focused but covers Anthropic-adjacent strategy; occasional engineer interviews |
| **Practical AI** | Daniel Whitenack + Chris Benson | Workflow-shaped; covers CC alongside Aider/Cursor/Codex |

## Newsletters / RSS-shaped written cadence

| Source | Angle | Cadence |
|---|---|---|
| **Ben's Bites** | Daily AI roundup; surfaces CC-relevant items fast | Daily |
| **AI Engineer (Swyx)** | Engineering practice across LLM tools; bigger picture than just CC | Weekly-ish |
| **Importance of Being Earnest** (Eugene Yan) | Practitioner-engineer reflection on ML/LLM eng — not CC-specific but the lens is right | Weekly-ish |
| **Hamel Husain** (Substack) | Sharp commentary on ML engineering reality; intermittently covers CC | Irregular |
| **Latent Space newsletter** | The newsletter version of the podcast; transcripts + companion essays | 1–2/week |
| **ClaudeLog changelog** | Third-party CC release tracking with editorial summaries | Per release |

## Aggregators / automated tracking

| Source | What |
|---|---|
| **★ Releasebot — Anthropic updates** | <https://releasebot.io/updates/anthropic/claude-code> — automated changelog with diff-format excerpts; useful when you want a single feed |
| **★ tonsofskills.com** | Skills + plugins + agents aggregator (425 + 2,810 + 200 as of 2026-05) — <https://github.com/jeremylongshore/claude-code-plugins-plus-skills> |
| **claudemarketplaces.com** | Cross-marketplace skill / plugin / MCP server directory; updates daily from GitHub |
| **★ agentskills.io** | The open-standard canonical list; cross-vendor skill index — <https://www.agensi.io/learn/claude-code-plugin-marketplace-guide> |
| **★ anthropics/claude-plugins-official** | The Anthropic-managed marketplace repo — watch releases for new-plugin signal |
| **★ netresearch/claude-code-marketplace** | Community-curated marketplace; agentskills.io reference implementation |

## Communities (real-time / Discord-shaped)

| Community | Where | Note |
|---|---|---|
| Latent Space Discord | Invite via latent.space | Highest-signal practitioner channel for cross-tool agentic engineering |
| Anthropic developer Discord | Via Anthropic platform docs | Slower than Latent Space; closer to product-team feedback |
| AI Engineer community | Per Swyx's newsletter | Adjacent — overlaps heavily with Latent Space |

## Adjacent ecosystems (peripheral-watch)

These aren't Claude Code primary, but their movements predict where
CC goes next or carry techniques worth borrowing.

| Source | What | Why we watch |
|---|---|---|
| **★ Paul Gauthier (Aider)** | <https://aider.chat/> + GitHub | Parallel-evolution CLI agent. The TDD-with-AI patterns matured here first. |
| **★ Cognition / Devin team** | <https://www.cognitionus.com/blog> | Longer-horizon-agent patterns; the *claude-memory-compiler* post is one example we cite |
| **Cursor team** | <https://cursor.com/blog>, X/Twitter | IDE-side framing of the same workflows; their rules system is the spiritual sibling of CLAUDE.md |
| **Continue.dev** | <https://continue.dev/blog> | Open-source IDE-agent — useful comparison for how skills + rules translate across surfaces |
| **OpenAI Codex CLI** | Repo + docs | The other big CLI-agent stack; cross-vendor skill compatibility starts here |
| **Gemini CLI** | Google docs | Third leg of cross-vendor skills standard |
| **GitHub Copilot (Agent mode)** | Copilot docs / GitHub blog | Distribution scale — what the eventual mass-market take looks like |

## Academic / paper-shaped

| Source | What |
|---|---|
| **★ arxiv cs.AI** — agent-systems papers | <https://arxiv.org/list/cs.AI/recent> — irregular but the VILA-Lab paper landed here; worth a weekly skim of recent agent-architecture submissions |
| **VILA-Lab** | <https://github.com/VILA-Lab/Dive-into-Claude-Code> — the team behind the source-leak analysis; likely to keep producing CC-internals work |

## What we're *not* yet watching (gaps to fill)

Honest about the holes:

- **CN-language CC writing**. There's substantial Chinese-language
  CC content (Substack-equivalents, Bilibili videos, Zhihu posts)
  that we're not pulling from yet. zeuikli's `cc-workspace-docs`
  was one entry point into that ecosystem.
- **JP-language CC writing**. Less surface area but high-signal —
  Japanese dev community has historically been early on agent
  patterns. Not currently in our feed.
- **Discord channel notes**. The Latent Space / Anthropic Discords
  carry real conversation that doesn't make it to blog form for
  weeks. We don't have a discipline yet for distilling those.
- **Twitter/X threads from specific named operators** — Karpathy
  threads on agent design, Yi Tay on model behaviour, Hamel Husain
  on ML eng practice. Cite when load-bearing; don't follow
  systematically. A real practice would.
- **Anthropic Slack / official-customer channels** — if you're an
  enterprise customer there are private channels with much earlier
  signal than the public blog. Not applicable to us at TinySmile
  scale today.

## How we update this file

Same rules as the entries: add new sources by appending to the
relevant section; mark with **★** once we've cited them in an
entry. When a source goes quiet for >6 months, move it to a
"former" section rather than deleting — readers should see the
ecosystem's churn, not just its current state.

When a source publishes something we want to cite, the workflow is:

1. Add the link inline at the point of claim in the relevant entry
2. Make sure the source appears in this file with the **★** marker
3. If new to this file, add it under the right section with the
   "what to expect" line

Don't cite sources here that aren't in any entry. The list earns
its place through use.
