# Claude Code in 2026 — a newest-first reference

Whittlery's running, dated reference on Claude Code as it evolved through
2026. Six entries today, newest at the top; the layout is intentionally
changelog-shaped so new material can be prepended without restructuring.

This exists because the most-cited 2026 community guides (zeuikli's
`cc-workspace-docs`, several Medium and Substack posts) have plausible
structure but factual errors that hide behind being in your second
language. The remedy here: every concrete claim is cited, every event
name verified against the [official Anthropic
changelog](https://code.claude.com/docs/en/changelog), and the entries
are dated so a reader can see which parts age first.

## Entries (newest → oldest)

| Month | File | One-line |
|---|---|---|
| **2026-05** | [`2026-05-code-review-and-conference.md`](./2026-05-code-review-and-conference.md) | Code w/ Claude conference; the Code Review launch |
| **2026-04** | [`2026-04-managed-agents-sdk.md`](./2026-04-managed-agents-sdk.md) | `managed-agents-2026-04-01` beta — multi-instance orchestration |
| **2026-03** | [`2026-03-source-leak-architecture.md`](./2026-03-source-leak-architecture.md) | The npm-missing-`.npmignore` leak — what 512K lines of CC source actually says |
| **2026 (ongoing)** | [`2026-hooks-and-effort.md`](./2026-hooks-and-effort.md) | Hooks evolved through 2026: `effort.level`, `terminalSequence`, `duration_ms`, the 8-block `Stop` cap |
| **2026** | [`2026-skills-and-marketplace.md`](./2026-skills-and-marketplace.md) | Skills + plugin marketplace + the agentskills.io cross-vendor open standard |
| **2026** | [`2026-claude-md-and-plan-mode.md`](./2026-claude-md-and-plan-mode.md) | Forrest Chang's CLAUDE.md (220K ⭐), plan mode, the spec-driven workflow |

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

## Contributing / updating

Add a new entry by writing `<YYYY-MM>-<slug>.md` and prepending one
line to the index above. Don't renumber. Don't backdate.

When a claim in an existing entry becomes wrong (a hook event renamed,
an SDK version superseded), edit in place with a strikethrough or a
"superseded 2026-XX-XX" note at the top of the affected section —
don't silently delete. Half the value here is letting readers see
*when* the shape of Claude Code changed.

## License

MIT. See [LICENSE](./LICENSE).

## About

Maintained under the Genno persona at
[whittlery.io](https://whittlery.io). Companion to
[`genno-whittlery/web-monkey`](https://github.com/genno-whittlery/web-monkey)
(in-house UI fuzz harness, also OSS).
