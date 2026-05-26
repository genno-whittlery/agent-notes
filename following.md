# Who we're following — intake list for Claude Code, 2026

Two reasons to keep up with a Claude Code source:

1. **Helps users** — practical tips, configs, workflows, templates
   that make your next CC session better.
2. **Big news** — releases, paradigm shifts, leaks, conference
   launches; anything notable enough to discuss broadly.

The list below is organised that way. Entries marked **★** are
already cited in one of the six entries.

Same shape as the entries themselves: dated when possible, honest
about what each source covers and doesn't, no implicit ranking.

---

## Helps users

Sources whose primary value is *making your next session better* —
workflow recipes, templates, skill / plugin catalogs, debugging
patterns, configs to copy.

### Anthropic first-party — the user-facing surfaces

| Source | What | Where |
|---|---|---|
| **★ Claude Code docs** | Reference for CLI / config / settings.json / MCP wiring | <https://code.claude.com/docs/en/> |
| **★ Anthropic best-practices** | Anthropic's own "how to use Claude Code well" page — the canonical opinionated guide | <https://code.claude.com/docs/en/best-practices> |
| **★ Claude Cookbook** | Worked examples for SDK + agent patterns; growing on every launch | <https://platform.claude.com/cookbook/> |

### Workflow demos + recipes

| Source | What |
|---|---|
| **★ Cole Medin (YouTube)** | Long-form CC demos; the 24-hour-coding-agent video is a category-defining example — <https://www.youtube.com/@ColeMedin> |
| **★ Scott Spence** | Practical write-ups on plugin marketplaces + skill organisation — <https://scottspence.com/> |
| **★ Mark Chen (Medium)** | Beginner-friendly walkthroughs; the skills-marketplace post is the cleanest intro to plugin install flow — <https://medium.com/@markchen69> |
| **★ Wataru Takahashi** | Spec-driven-development series; deep on the spec→plan→code workflow — <https://watarutakahashi.substack.com/> |
| **★ DataCamp tutorials** | Series on plan mode, TDD with Claude, spec-driven dev — <https://www.datacamp.com/tutorial/spec-driven-development-with-claude-code> |
| **★ firecrawl blog** | "Best Claude Code Skills to Try in 2026" — useful for skill discovery — <https://www.firecrawl.dev/blog/best-claude-code-skills> |
| **★ BuildFastWithAI** | The complete 2026 skills guide; tutorial-shaped — <https://www.buildfastwithai.com/blogs/claude-skills-complete-guide-2026> |
| **★ Nimbalyst** | Practical CC guides (skills + subagents) — <https://nimbalyst.com/blog/> |
| **Hamel Husain (Substack)** | Practitioner-engineer commentary; intermittent but high-signal when he writes about CC |
| **Eugene Yan — *Importance of Being Earnest*** | ML/LLM-eng practitioner reflection; not CC-specific but the lens is right |

### Templates + catalogs

| Source | What |
|---|---|
| **★ Forrest Chang's CLAUDE.md** | The 220K+ ⭐ fork-network anchor — the most-cited CLAUDE.md template in existence (works as starting point even if you write your own) |
| **★ anthropics/claude-plugins-official** | The Anthropic-managed plugin marketplace — <https://github.com/anthropics/claude-plugins-official> |
| **★ agentskills.io** | Cross-vendor open-standard skill index — <https://www.agensi.io/learn/claude-code-plugin-marketplace-guide> |
| **★ tonsofskills.com / `jeremylongshore/claude-code-plugins-plus-skills`** | Largest aggregator (425 plugins, 2,810 skills, 200 agents as of 2026-05) |
| **claudemarketplaces.com** | Cross-marketplace skill / plugin / MCP server directory; updates daily from GitHub |
| **★ `netresearch/claude-code-marketplace`** | Community-curated marketplace; agentskills.io reference implementation |

### Practitioner conversations

| Source | What |
|---|---|
| **★ Latent Space** (podcast + newsletter) | Highest-signal cross-vendor agentic-engineering channel; Swyx + Alessio gather the operators — <https://latent.space/> |
| **Latent Space Discord** | Real-time practitioner channel where the workflow patterns get debated weeks before they hit blogs |

---

## Big news

Sources whose primary value is *catching what just happened* —
release notes, paradigm-shift posts, leak coverage, conference
launches.

### Official release channels

| Source | What | Cadence |
|---|---|---|
| **★ Anthropic Claude Code changelog** | Per-version notes; canonical for hook events / flag changes / env vars | <https://code.claude.com/docs/en/changelog> — every 1–2 days during 2026 |
| **GitHub releases — `anthropics/claude-code`** | Raw release feed; arrives *before* the changelog page renders. RSS-able. | <https://github.com/anthropics/claude-code/releases> |
| **Anthropic News / Product** | Launch announcements (Code Review, Managed Agents, conference recaps) | <https://www.anthropic.com/news> |
| **@AnthropicAI on X/Twitter** | Same launches, faster surfacing during live events | [twitter.com/AnthropicAI](https://twitter.com/AnthropicAI) |
| **Code w/ Claude conference** | Anthropic's annual developer event — first edition May 2026 (Code Review launch). Recorded talks land on YouTube after | <https://www.anthropic.com/events> + [Anthropic YouTube](https://www.youtube.com/@anthropic-ai) |
| **★ Releasebot — Anthropic** | Third-party automated changelog with diff-format excerpts; useful as a single feed across products | <https://releasebot.io/updates/anthropic/claude-code> |
| **ClaudeLog third-party changelog** | Editorial summaries on top of the raw changelog — useful when you want "what does this version mean for me" rather than the raw entries |

**Practical note:** Anthropic-side, the *GitHub releases* page
arrives first, the *changelog page* catches up 24–48 hours later
with prose. If you want "the moment a new version ships," the
GitHub releases RSS is the right feed; for the digested version,
the changelog page.

### Direction-of-travel voices (paradigm shifts, opinion-setting)

| Source | What |
|---|---|
| **★ Anthropic Engineering blog** | The post-leak *Effective context engineering for AI agents* piece is the foundational 2026 framing of "prompt engineering is over, context engineering is here" — <https://www.anthropic.com/engineering> |
| **★ Boris Cherny** (Head of Claude Code) | The most-cited single voice on CC in 2026; podcast circuit + [@bcherny](https://twitter.com/bcherny). The "coding is solved, what comes next" framing is his |
| **★ Cat Wu** (founding engineer) | Engineering-side framing; tools & UX choices. Co-appears with Cherny on big interviews |
| **★ Simon Willison** | Live-blogs Anthropic events; cited deep dives on CC behaviour — <https://simonwillison.net/> |
| **Andrej Karpathy** | The "80% agent-driven coding" pivot post inspired the Forrest Chang CLAUDE.md pattern; not a regular CC commentator but his occasional pieces move the conversation |

### Event coverage + investigative

| Source | What |
|---|---|
| **★ VILA-Lab — *Dive into Claude Code*** | Academic analysis of the source leak; arxiv 2604.14228 + follow-up posts — <https://github.com/VILA-Lab/Dive-into-Claude-Code> |
| **★ Layer5 engineering blog** | The single best deep-dive on the source leak (file counts, QueryEngine analysis) — <https://layer5.io/blog/engineering/the-claude-code-source-leak-512000-lines-a-missing-npmignore-and-the-fastest-growing-repo-in-github-history/> |
| **★ Cognition Labs blog** | Cross-ecosystem big-news items (the *claude-memory-compiler* post was the first serious Karpathy-wiki implementation for CC) — <https://www.cognitionus.com/blog> |

### Podcast circuit (where Anthropic's framing gets articulated)

| Show | Hosts | Why listen |
|---|---|---|
| **★ The Pragmatic Engineer** | Gergely Orosz | Cherny's March 2026 appearance is the most cited single CC interview — engineering-management lens |
| **★ Every / AI & I** | Dan Shipper | Cherny + Wu joint appearance (October 2025) was the foundational "how we use CC inside Anthropic" interview |
| **★ Lenny's Newsletter Podcast** | Lenny Rachitsky | Cherny "what comes after coding is solved" — product-management lens |
| **No Priors** | Sarah Guo + Elad Gil | Anthropic-adjacent strategy; occasional CC-relevant interviews |

---

## Peripheral — adjacent ecosystems, when relevant

These don't fit the "helps users" or "big news" buckets directly —
they're parallel-evolution agent tools whose movements occasionally
predict where CC goes or carry techniques worth borrowing. Watch
when actively relevant; don't follow on default cadence.

| Source | What | Why peripheral-watch |
|---|---|---|
| **★ Paul Gauthier (Aider)** | <https://aider.chat/> | TDD-with-AI patterns matured here first |
| **Cursor blog** | <https://cursor.com/blog> | Their rules system is the spiritual sibling of CLAUDE.md |
| **Continue.dev blog** | <https://continue.dev/blog> | Open-source IDE-agent; useful for cross-surface skill comparison |
| **OpenAI Codex CLI / Gemini CLI / GitHub Copilot Agent mode** | Respective vendor docs | Cross-vendor skill compatibility starts here; agentskills.io spans all of them |

---

## What we're *not* yet watching (gaps to fill)

Honest about the holes:

- **CN-language CC writing.** Substantial Chinese-language CC content
  on Bilibili / Zhihu / WeChat that we're not pulling from yet.
- **JP-language CC writing.** Less surface area but high-signal —
  the Japanese dev community has been early on agent patterns.
- **Discord channel notes.** The Latent Space / Anthropic Discords
  carry real conversation that doesn't make it to blog form for
  weeks. No discipline yet for distilling those.
- **Specific operator threads on X** — Karpathy, Yi Tay, Hamel
  Husain. Cite when load-bearing; don't follow systematically yet.
- **Enterprise / private channels.** Anthropic Slack channels for
  enterprise customers have earlier signal than public surfaces.
  Not applicable at TinySmile scale today.

---

## How we update this file

Same rules as the entries: add new sources by appending to the
relevant section; mark with **★** once we've cited them in an
entry. When a source goes quiet for >6 months, move it to a
"former" section rather than deleting — readers should see the
ecosystem's churn, not just its current state.

When a source publishes something we want to cite, the workflow:

1. Add the link inline at the point of claim in the relevant entry
2. Make sure the source appears in this file with the **★** marker
3. If new to this file, add it under the right section with the
   "what to expect" line

Discipline: don't add a source here unless we'd actually use it.
The list earns its place through use, not aspirational following.
