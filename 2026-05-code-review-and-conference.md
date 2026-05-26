# 2026-05 — Code Review and the *Code w/ Claude* conference

## Why this matters

May 2026 is when Anthropic stopped framing Claude Code as a CLI coding
assistant and started framing it as a *platform*. The framing shift
happened at *Code w/ Claude 2026* — Anthropic's own developer
conference, live-blogged by Simon Willison on 2026-05-06. The headline
launch was **Code Review**, a feature that Anthropic confirmed is
"used by every team at Anthropic."

The substance behind the framing: in 2026 the dominant question stopped
being "can Claude write this function" and became "what's the harness
around Claude that catches its mistakes." Code Review is the most
visible answer Anthropic has shipped for that question yet.

## What Code Review actually is

Code Review is a per-PR / per-change automated review step that runs a
Claude agent against the diff with a defined checklist, posts comments
back on the PR, and (depending on configuration) blocks merge until
issues are addressed. The novelty isn't "AI reviews code" — that's
been around in various forms — it's that the review agent is
*specifically a Claude Code agent*, so it inherits the same harness,
the same skills, the same hooks, and the same CLAUDE.md context as the
human-facing Claude Code session that wrote the change.

In other words: the reviewer and the author are running the same
stack with different intent. That's load-bearing for the platform
framing — the harness is supposed to be one thing, not "the IDE
edition" + "the review edition" + "the CI edition."

## The "coding is solved" framing

The conference was the loudest articulation of a thesis Boris Cherny
(Head of Claude Code) and Cat Wu had been developing across podcasts
for the prior two months — *Pragmatic Engineer* (March 2026), *Lenny's
Newsletter* (February 2026), the *Every / AI & I* podcast appearance.
The thesis in one line: **the bottleneck is no longer code generation;
it's the loop around it.**

Concrete consequences they articulated, paraphrasing across the
podcast appearances:

- *Parallel agents* — running multiple Claude Code instances on
  different branches of the same problem is a real workflow now, not
  an experiment.
- *PR structure as compiler input* — how you decompose a change into
  PRs is now a load-bearing decision for what Claude can review well.
- *Deterministic review patterns* — the review checklist itself is
  worth engineering, separately from the prompt that wrote the code.
- *"Claude Cowork"* — Cherny referenced this on Pragmatic Engineer as
  an internal collaboration pattern around the agent. Public details
  thin at time of writing.

The slogan that got quoted everywhere from the conference: *"coding
is solved, what comes next."* It's hyperbole as a literal claim, but
it accurately marks the boundary the conference framed: the agent
*alone* is no longer interesting; the *system around the agent* is.

## What changed in the harness around the same time

Several adjacent changes landed near the May 2026 conference that
made Code Review viable as more than a marketing screenshot:

- **`PostToolUseFailure` event** — a hook event that fires only when
  a tool call failed. Lets a review pipeline observe failed-tool
  patterns without re-implementing failure detection on the consumer
  side.
- **`duration_ms` in `PostToolUse` / `PostToolUseFailure` hook
  input** — shipped in 2.1.119. Excludes time spent in permission
  prompts and PreToolUse hooks. Lets a reviewer agent reason about
  performance of the change it's reviewing.
- **`Stop` hook 8-consecutive-block cap** — shipped in 2.1.143. Stops
  a misbehaving review pipeline from infinitely re-blocking the
  authoring agent. Pragmatic safety rail.
- **Plugin component inventory in `claude plugin details`** — the
  CLI surfaces what a plugin contributes (commands / agents / skills
  / hooks / MCP servers) and its projected per-session token cost
  *before* you install. Reviewer agents can be packaged as plugins
  and audited the same way.

These aren't headline features individually; together they're the
plumbing that makes the Code Review story load-bearing.

## Try this

If you've installed Claude Code and have a Git repo you're willing to
let Claude touch:

1. Inside Claude Code, type `/plugin` to open the plugin browser.
   Filter for `code-review` or `review`. The Anthropic-managed
   marketplace ships a review plugin; community marketplaces (e.g.
   the netresearch one) ship variants.
2. Install one. Inspect the plugin's contributed components — what
   hooks does it register, what agents does it dispatch, what skills
   does it activate. `claude plugin details <plugin-name>` shows the
   inventory now.
3. Make a small change on a branch. Ask Claude `review this branch`.
   Watch which hook events fire (you can register your own
   `PostToolUseFailure` hook that just logs to a file as an
   observation tool) and how the review agent compares to the
   authoring agent in terms of context loaded.

The interesting bit isn't whether the review catches a bug. It's
seeing that the *reviewer is running the same harness as the author*,
with a different system prompt. That's the platform claim.

## Sources

- Simon Willison, *Live blog: Code w/ Claude 2026* (2026-05-06):
  <https://simonwillison.net/2026/May/6/code-w-claude-2026/>
- Boris Cherny on *The Pragmatic Engineer* (March 2026), discussing
  Claude Cowork, parallel agents, PR structure, deterministic review:
  <https://open.spotify.com/episode/3fuz0q5vlIQE1DB6JmKZ8B>
- Boris Cherny on *Lenny's Newsletter* (Feb 2026), "What happens
  after coding is solved":
  <https://www.lennysnewsletter.com/p/head-of-claude-code-what-happens>
- *Every / AI & I* podcast, *How to Use Claude Code Like the People
  Who Built It* — Boris Cherny + Cat Wu:
  <https://every.to/podcast/how-to-use-claude-code-like-the-people-who-built-it>
- Anthropic, *Claude Code product page* (Code Review feature):
  <https://www.anthropic.com/product/claude-code>
- Anthropic changelog entries for 2.1.119 (`duration_ms`), 2.1.143
  (`Stop` 8-cap): <https://code.claude.com/docs/en/changelog>
