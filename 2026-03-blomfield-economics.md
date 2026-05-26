# 2026-03 — Tom Blomfield on the economics of agentic coding

## Why this matters

Tom Blomfield (Monzo founder; now General Partner at Y Combinator)
delivered the highest-reach 2026 articulation of the
*economics-of-AI-coding* thesis. The quote that travelled
furthest:

> "The entire Accenture workforce is about to be outperformed by a
> 24-year-old who learned Claude Code last Tuesday."

The line is hyperbolic on its face. What makes it weight-bearing
is the **observation base** behind it: as a YC partner, Blomfield
sees hundreds of startups' inside view. Specifically:

> "About a quarter of YC founders told [me] that 95% of their code
> is written by AI."

That number — independently corroborated by *Anthropic's own
internal numbers*, where cofounder Jack Clark said in February 2026
that Claude writes "comfortably the majority" of Anthropic's code
and "could handle nearly all of it by year-end if current progress
continues" — is the load-bearing claim. Once you accept that
data, Blomfield's stronger framings (5-engineer teams matching
50-engineer output; software engineering's existence-shape
changing in 6-12 months) become arguable rather than absurd.

Where this fits in this notes set: the rest of the repo is
*how* you make Claude Code do useful work (harness, hooks,
plan mode, skills, subagents). Blomfield's talk is the *why is
this load-bearing*. The economics frame motivates the engineering
discipline.

## What Blomfield actually said

Across his LinkedIn posts, conference appearances, and the YC
content circuit through 2026-Q1, the recurring claims:

1. **"95% AI-written" at the small-startup end.** About a quarter
   of YC founders self-report this to him. The other three-
   quarters are presumably somewhere lower but rising. The
   distribution is the interesting thing — the *frontier* of
   AI-written-code-proportion is no longer a research demo; it's
   already shipping production startups.

2. **Teams of 5 doing what used to take 50.** Cursor, Windsurf,
   Lovable cited as $100M+ revenue companies with "astonishingly
   small teams." This isn't a future claim; it's a current
   observation. The leverage already exists.

3. **"Software engineering won't exist in the same way"** — in 6
   to 12 months. The qualifier is doing work: the *profession*
   may persist; the *daily activity* of typing-code-into-an-IDE
   shifts toward something else (specification authoring, review,
   verification, harness design).

4. **The Accenture line specifically.** It's a barbed framing
   aimed at the *large-services-firm-billing-model* — work that's
   priced as "humans at scale" is exactly the kind that an
   individual with Claude Code can underbid. Whether or not the
   24-year-old literally outperforms the entire workforce, the
   *unit economics of consulting* shift before the *technical
   ceiling* of agentic coding does.

## Cross-reference: Anthropic's own self-report

The Blomfield observations land harder because *Anthropic itself*
publicly confirmed adjacent numbers in 2026. From Jack Clark in
February 2026, paraphrased across multiple writeups:

- Claude writes "comfortably the majority" of Anthropic's code
- This trajectory was "on track for nearly all of it" by year-
  end if then-current rates held
- Used by every team internally — not a research demo, daily
  driver across the company

This matters because Anthropic isn't a YC startup with selection
bias. It's a sophisticated AI lab building the agent itself. If
*they* are using Claude Code to write Claude Code's own code,
the regression test on Blomfield's broader claim is internal.

## What this means for someone working with Claude Code

If you accept the economics frame — and the data forces some
acceptance even if not the strongest version — the practical
consequences:

1. **Your skill investment in CC compounds quickly.** The
   leverage is bigger than the leverage you had with previous
   dev-productivity tools (IDEs, version control, search). The
   *return on a week spent learning Claude Code well* is larger
   than the equivalent week spent learning Git was in 2010.
2. **Harness design is the moat.** If everyone has access to the
   same model, the differentiator is what surrounds it — the
   CLAUDE.md, the skills, the hooks, the subagent patterns. See
   the [harness engineering entry](./2026-harness-engineering.md)
   for why this is The Argument of 2026.
3. **The market for "agent-assisted" labour is bifurcating.**
   At one end: solo operators and 5-engineer teams reaching
   $100M ARR by deploying agentic-coding leverage to the hilt.
   At the other: large services firms (Accenture, Infosys,
   Capgemini) selling priced-by-headcount engagements that are
   becoming structurally uncompetitive against the same work
   done by smaller teams using agents.
4. **Skill churn is real and continuous.** What you learned
   about Claude Code six months ago is partly outdated now (this
   notes set exists in part to track that). Plan to relearn,
   not to plateau.

## Where Blomfield himself ends up

A useful framing he repeats: the *founder skill set* is what
benefits most from AI coding leverage. Founders historically
had to defer specific implementation choices to engineers
because typing-out-the-code was a real constraint on time. With
that constraint relaxed, founders can prototype, iterate, and
ship product changes themselves. The bottleneck moves upstream
to *what to build*, not *who can build it*.

This is consistent with the YC-portfolio observation: at the
seed stage, founders with high taste in product + high comfort
with agentic tooling are *already* outshipping teams that have
better engineers but slower iteration cycles.

## Try this

Three small experiments to calibrate against the Blomfield
claims for your own situation:

1. **Track the ratio.** For one week, after each code change you
   ship, log whether *you* typed the bulk of it or *Claude Code*
   did. After a week, you'll have a number. If you're below the
   YC-startup 95% median, ask why — is it because your codebase
   is genuinely harder, because your CLAUDE.md isn't tuned, or
   because you're not yet trusting the agent for work it would
   actually do well?
2. **Time a feature both ways.** Pick a small feature. Implement
   it with CC in plan mode + verification (spec → plan → code
   with reviews); separately, estimate how long it would have
   taken you typing the same feature manually. The delta is your
   leverage number.
3. **Read Blomfield's posts directly.** His
   [LinkedIn](https://www.linkedin.com/in/tomblomfield/) feed has
   the unfiltered version; the press writeups quote the most
   inflammatory lines but lose the qualifiers.

## Sources

- [Tom Blomfield's website](https://tomblomfield.com/) — primary
  source for his writing
- [Tom Blomfield — YC Partner profile](https://www.ycombinator.com/people/tom-blomfield)
- [Tom Blomfield on LinkedIn](https://www.linkedin.com/in/tomblomfield/)
  — where the most-quoted posts originate
- *AI Shockwave: Can One Claude User Outpace a Corporate Giant?* —
  The Hans India:
  <https://www.thehansindia.com/technology/tech-news/ai-shockwave-can-one-claude-user-outpace-a-corporate-giant-1053001>
- *Tom Blomfield's Claude AI comment fuels concerns over future
  of Accenture and IT services* — Storyboard18:
  <https://www.storyboard18.com/brand-makers/tom-blomfields-claude-ai-comment-fuels-concerns-over-future-of-accenture-and-it-services-91120.htm>
- *According to a Y Combinator partner, Claude AI will allow a
  24-year-old to outperform the entire Accenture staff* —
  TheSwipeUp (2026-03):
  <https://www.theswipeup.com/2026/03/according-to-y-combinator-partner.html>
- Jack Clark / Anthropic public statements on internal coding
  use (Feb 2026) — referenced across multiple outlets covering
  the Blomfield posts
