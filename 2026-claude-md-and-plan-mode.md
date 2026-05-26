# 2026 — CLAUDE.md and plan mode

## Why this matters

If the rest of the stack is the platform, CLAUDE.md is the contract.
It's the single piece of Claude Code that every team configures, and
the 2026 community consensus — across firecrawl, buildfastwithai,
the Anthropic engineering blog itself, and Forrest Chang's
220,000-star configuration repo — is that **CLAUDE.md is as
important as `.gitignore`**. Essential infrastructure, not optional
documentation.

The other piece in this entry — *plan mode* and the spec-driven
workflow that grew around it — is the most viral 2026 *usage*
pattern. It's the answer to "the agent went off the rails because I
gave it a one-sentence brief." Whatever your CLAUDE.md says, what
the agent does next turn is set by *the plan*.

These two — the standing instructions (CLAUDE.md) and the
turn-specific plan (plan mode) — are the foundations everything in
the newer entries (managed agents, code review, hooks, skills) is
built on. That's why they're at the bottom of this newest-first
reference: you read up from here.

## CLAUDE.md: the hierarchy

CLAUDE.md is loaded by Claude Code at session start and inserted
into the system prompt. The 2026 hierarchy, in order of precedence
(later overrides earlier):

| File | Scope | What goes here |
|---|---|---|
| `~/.claude/CLAUDE.md` | User-global | Your name, role, communication preferences, tools you always use |
| `<project>/CLAUDE.md` | Project | Project-specific build commands, code style, architecture, what NOT to touch |
| `<project>/CLAUDE.local.md` | Project, gitignored | Personal-to-you, in-project overrides; not committed |

Plus: `--add-dir <path>` extends what counts as "the project" — any
CLAUDE.md inside the added directory is also loaded.

The leak revealed an implementation detail worth knowing:
**CLAUDE.md is loaded once per session, near the top of the system
prompt.** Editing it mid-session has no effect until you `/init` or
restart. This is the difference between "Claude is ignoring my
CLAUDE.md" (it isn't — you edited it after the session started) and
"Claude is misreading my CLAUDE.md" (the actually-rare case).

## The Forrest Chang four-rule pattern (220K ⭐)

Forrest Chang's `CLAUDE.md` repo, published 2026-01-27 and inspired
by Andrej Karpathy's "80% agent-driven coding" framing, accumulated
220,000+ combined GitHub stars across forks by mid-2026. It's the
most-copied single CLAUDE.md template in existence. The headline
contribution: four rules that, in Chang's framing, *stop AI from
breaking code*.

Paraphrased shape (the actual rules vary by fork; the pattern is
durable):

> 1. **Plan before code.** No edits before the user agrees to a
>    plan. If the user says "just do it," ask once more in the form
>    of a concrete plan, then proceed.
> 2. **One change, one place.** Don't refactor while fixing a bug.
>    Don't add features while refactoring. The scope is the brief,
>    not what you noticed while reading.
> 3. **Verify before claiming.** Don't say "fixed" without running
>    something that proves it. Tests, build, lint, manual check —
>    whatever the project supports.
> 4. **Surface uncertainty.** When you don't know, say so. When
>    you guess, say "I'm guessing." The user is the decision-maker
>    on ambiguity.

These four rules are notable because they're not technology-specific.
They survive switching languages, frameworks, even agents (the same
rules work in Codex / Cursor / Gemini CLI). They're a *process*
guarantee, not a syntactic one — which is why they hold up.

## Plan mode

Plan mode is invoked via `/plan` and is the implementation of rule
1 from the Chang pattern. The flow:

1. User invokes `/plan` (or appends `"think hard"` / `"ultrathink"`
   to the prompt — triggers that the harness interprets as plan-mode
   intent without the explicit slash command).
2. The harness **dispatches a sub-agent specialised in research**.
   The sub-agent reads the codebase, gathers context, and reports
   back to the main agent.
3. The main agent writes a `plan.md` file to disk — a step-by-step
   breakdown of what it intends to do.
4. The user reviews the plan. Edits, accepts, rejects.
5. Only on user acceptance does the main agent start executing.
   Tasks happen *one at a time*, with the human reviewing between
   each step.

The hierarchy of guarantees is important:

- **No execution before plan.** The harness blocks tool calls that
  would modify the project until the user has approved a plan.
- **One task at a time after acceptance.** Even after approval, the
  agent doesn't fire off all the tasks in parallel; it does the
  first one, reports, waits, does the next.
- **Human review between tasks.** The "review" is whatever you
  define it as — could be `:wq` on a generated test file, could be
  a one-line "OK." The point is the harness pauses for it.

## Spec-driven development

A pattern that emerged out of plan mode in early 2026 and got
[Hacker News thread #48231575](https://news.ycombinator.com/item?id=48231575)
+ DataCamp + multiple Medium writeups. It's plan mode extended one
step backwards: **before the plan, write a spec**.

The full flow becomes:

1. **Spec** — a `spec.md` describing *what* the change should do.
   Written by the user with light Claude assistance, NOT by Claude
   alone. The spec is the contract the rest of the flow honours.
2. **Plan** — Claude reads the spec, dispatches a research sub-agent
   if needed, writes a `plan.md` describing *how* to implement
   what's in the spec. Reviewed by user.
3. **Code** — Claude writes code against the plan, one task at a
   time, with a human review between each pair of tasks.

The viral framing: **spec → plan → code, with a human review between
every pair of documents.** Three documents, two reviews. The
discipline is what stops scope creep.

Why this works in 2026 and didn't really in 2025: the model's
ability to *honour* a multi-page spec without drifting got
materially better in 2026 (specifically with Opus 4.x), and
managed-agent dispatch (April 2026) made the research sub-agent
step robust rather than experimental.

## The "ultrathink" trigger

In 2026 the community settled on a small set of trigger phrases the
harness recognises as plan-mode intent without an explicit `/plan`:

- `think hard`
- `think hard about this`
- `ultrathink`
- `think step by step`

These aren't magic — they're patterns the harness's natural-language
matcher correlates with plan-mode dispatch. They work because
**the harness specifically watches for them**, not because LLMs
respond to the literal phrase. Don't rely on them in scripted
contexts where reliability matters; use `/plan` explicitly.

## A minimum-viable 2026 CLAUDE.md

The 2026 community consensus shape, distilled:

```markdown
# Project CLAUDE.md

## What this project is

One paragraph. Tech stack, what it does, who uses it.

## Commands

- `npm run dev` — start dev server
- `npm run test:run` — run tests once
- `npm run build` — production build

## Code style

- TypeScript strict. No `any`.
- Tests live in `test/`, not co-located.
- Prefer pure functions; isolate side effects in `services/`.

## Things to do

- Plan before coding. Show me the plan; wait for "ok."
- Verify before claiming. Run tests after edits.
- Surface uncertainty. If you're guessing, say so.

## Things NOT to do

- Don't touch `legacy/` — it's frozen and shipped to production.
- Don't run `--no-verify` on git operations.
- Don't add features while fixing a bug. Different PRs, different
  conversations.

## When you're stuck

If a build is failing and the cause isn't obvious in 5 minutes,
stop and ask. Don't keep changing things hoping it'll work.
```

That fits on one screen, covers the Chang four-rule essentials, and
gives the agent concrete enough constraints that "go off the rails"
becomes much harder.

## Try this

1. **Read your installed Claude Code's view of your CLAUDE.md.**
   Inside a session, type `/init` to surface the current
   CLAUDE.md hierarchy and what's loaded. Useful as a sanity check.
2. **Try plan mode on a small change.** Pick a 30-minute task. Use
   `/plan` (or end your prompt with "think hard about this").
   Watch the research sub-agent dispatch, watch the plan get
   written, edit the plan, accept, then watch the per-task review.
   Compare to the same task done without plan mode.
3. **Borrow Forrest Chang's pattern, don't fork it.** Take the
   four-rule spirit, write your own version that's specific to your
   project. The four rules generalise; *which* uncertainty surfaces
   matter to you, *what* "verify before claiming" looks like in
   your codebase — those are local. Copy the discipline; don't
   copy the text.

## Sources

- *Karpathy-Inspired CLAUDE.md Passes 220,000 Combined GitHub Stars
  With Four Rules That Stop AI Breaking Code* — TechTimes
  (2026-05-18):
  <https://www.techtimes.com/articles/316798/20260518/karpathy-inspired-claudemd-passes-220000-combined-github-stars-four-rules-that-stop-ai-breaking.htm>
- *Effective context engineering for AI agents* — Anthropic
  Engineering blog:
  <https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents>
- *Context engineering: memory, compaction, and tool clearing* —
  Anthropic Claude Cookbook:
  <https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools>
- *Show HN: Spec-Driven Development Workflow for Claude Code* —
  [Hacker News #48231575](https://news.ycombinator.com/item?id=48231575)
- *Spec-Driven Development with Claude Code: A Guided Tutorial* —
  DataCamp:
  <https://www.datacamp.com/tutorial/spec-driven-development-with-claude-code>
- *Plan-Driven Development* — DeepWiki on the
  FlorianBruniaux/claude-code-ultimate-guide:
  <https://deepwiki.com/FlorianBruniaux/claude-code-ultimate-guide/7.2-test-driven-development-with-claude>
- *Claude Code Best Practices: Planning, Context Transfer, TDD* —
  DataCamp: <https://www.datacamp.com/tutorial/claude-code-best-practices>
- Anthropic Claude Code docs — *Best Practices for Claude Code*:
  <https://code.claude.com/docs/en/best-practices>
