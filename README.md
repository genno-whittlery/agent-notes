# Claude Code in 2026 — a newest-first reference

*Rendered: <https://genno-whittlery.github.io/claude-code-notes/>*

Personal study notes on Claude Code as it evolved through 2026. Six
entries today, newest at the top; the layout is intentionally
changelog-shaped so new material can be prepended without
restructuring as our understanding grows.

Working discipline: every concrete claim cites a 2026 source, every
hook event name is verified against the
[official Anthropic changelog](https://code.claude.com/docs/en/changelog)
before going in, and entries are dated so a reader (future-us
included) can see which parts age first.

## Entries (newest → oldest)

Entry kind: **static** = a dated event whose facts don't move; **rolling**
= a topic that accretes references and gets supersede-notes as the
shape changes.

| Month | Kind | File | One-line |
|---|---|---|---|
| **2026-05** | static | [`2026-05-code-review-and-conference.md`](./2026-05-code-review-and-conference.md) | Code w/ Claude conference; the Code Review launch |
| **2026-05** | rolling | [`2026-05-ds4-c-local-inference.md`](./2026-05-ds4-c-local-inference.md) | antirez/ds4 — single-file C inference engine running DeepSeek V4 Flash locally; OpenAI/Anthropic-compatible API so Claude Code can use it as a backend |
| **2026-04** | static | [`2026-04-aie-europe.md`](./2026-04-aie-europe.md) | AI Engineer Europe (London, 8–10 April) — harness engineering crystallises, "strange stickiness of coding products," AI coding wars |
| **2026-04** | rolling | [`2026-04-hermes-agent.md`](./2026-04-hermes-agent.md) | Nous Research's Hermes Agent — CLI orchestrator that delegates to Claude Code + Codex; ACP protocol (April 2026) |
| **2026-04** | rolling | [`2026-04-managed-agents-sdk.md`](./2026-04-managed-agents-sdk.md) | `managed-agents-2026-04-01` beta — multi-instance orchestration |
| **2026-03** | rolling | [`2026-03-blomfield-economics.md`](./2026-03-blomfield-economics.md) | Tom Blomfield (YC partner) on the economics of agentic coding — the "24-year-old vs Accenture" line, 95%-AI-written at YC startups, Jack Clark's "majority of Anthropic's code" |
| **2026-03** | static¹ | [`2026-03-source-leak-architecture.md`](./2026-03-source-leak-architecture.md) | The npm-missing-`.npmignore` leak — what 512K lines of CC source actually says |
| **2026** | rolling | [`2026-harness-engineering.md`](./2026-harness-engineering.md) | The 2026 term-of-art for designing the layer around the LLM; two-agent + three-agent patterns; the 88% production-failure stat |
| **2026** | rolling | [`2026-hooks-and-effort.md`](./2026-hooks-and-effort.md) | Hooks evolved through 2026: `effort.level`, `terminalSequence`, `duration_ms`, the 8-block `Stop` cap |
| **2026** | rolling | [`2026-skills-and-marketplace.md`](./2026-skills-and-marketplace.md) | Skills + plugin marketplace + the agentskills.io cross-vendor open standard |
| **2026** | rolling | [`2026-claude-md-and-plan-mode.md`](./2026-claude-md-and-plan-mode.md) | Forrest Chang's CLAUDE.md (220K+ ⭐ across the fork network), plan mode, the spec-driven workflow |

¹ The leak itself is a static 2026-03-31 event; the entry carries
some 2026-05 supersede notes (e.g., 2.1.143 `Stop` 8-cap) marking
post-March reverberations.

## Who we follow

The intake list — bloggers, podcasts, aggregators, official release
channels, adjacent ecosystems — that feeds the entries above lives
in [`following.md`](./following.md). Same shape as the entries:
dated, honest about gaps, cross-marked with the citations in each
entry so readers can see which voices show up where.

## How to read this

Top → bottom = recency-first. If you're new to Claude Code, **start at
the bottom** (`2026-claude-md-and-plan-mode.md`) — the patterns there
are what everything else above is built on. If you're already running
CC daily, start at the top to catch the changes since you last paid
attention.

Each entry has the same shape:

- **Why this matters** — what the story is, who's writing about it
- **The technical detail** — concrete, citable claims
- **Configuration or code** — copy-pasteable artefacts where applicable
- **Try this** — one experiment you can run today to see the behaviour
- **Sources** — every link cited, dated when possible

## What's *not* here

- A from-zero install tutorial. The [official
  docs](https://code.claude.com/docs/en/) handle that; we don't
  duplicate them.
- Generic prompt-engineering advice. The 2026 consensus (and one of
  the entries below) is that prompt engineering has been displaced by
  *context engineering*; tips on phrasing have a short half-life.
- Anything that wasn't documented or written about in 2026. Older
  patterns get a passing mention only when they're the foundation a
  2026 idea builds on.

## Updating

Add a new entry by writing `<YYYY-MM>-<slug>.md` and prepending one
line to the index above. Don't renumber. Don't backdate.

When a claim in an existing entry becomes wrong (a hook event
renamed, an SDK version superseded), edit in place with a
strikethrough or a "superseded 2026-XX-XX" note at the top of the
affected section — don't silently delete. Half the value here is
letting future-us see *when* the shape of Claude Code changed.

## License

MIT. See [LICENSE](./LICENSE). Public so the notes are reachable
across machines and shareable when useful, not because they claim to
be authoritative.

## About

Personal study notes kept under the Genno persona at
[whittlery.io](https://whittlery.io). Companion to
[`genno-whittlery/web-monkey`](https://github.com/genno-whittlery/web-monkey)
(in-house UI fuzz harness, also kept as public notes).
