# 2026 (rolling) — How to find hot topics systematically

## Why this matters

The other entries in this repo each cover *what* — a topic, a
launch, a pattern. This one covers *how* — the systematic method
for finding which topics are worth writing entries about in the
first place. It's the meta-discipline that makes the rest of the
repo's currency possible.

Without a system, you end up with two failure modes: (1)
chasing every shiny launch and producing low-signal coverage,
or (2) missing major paradigm shifts because you're not looking
in the right places. The 2026 ecosystem moves fast enough that
both failure modes are common.

[`following.md`](./following.md) is the *output* of this
methodology — the source list. This entry is the *process*.

## The five-tier intake stack

A reliable 2026 information diet for agentic engineering has
**five tiers**, ordered by signal-to-noise (highest first) and
update cadence:

### Tier 1 — first-party release channels (highest signal)

What ships from the vendors themselves. Cadence: per-release,
typically daily for active products.

- **GitHub releases page** for the actual tool —
  `github.com/anthropics/claude-code/releases` is the rawest
  Anthropic signal; matches arrive there before the prose
  changelog catches up
- **Vendor changelog pages** —
  [Anthropic's](https://code.claude.com/docs/en/changelog),
  OpenAI's, etc.
- **Vendor X/Twitter accounts** for launch announcements
- **Vendor blogs** for paradigm-shift framing
  ([Anthropic Engineering](https://www.anthropic.com/engineering)
  is the load-bearing one for Claude Code)

**Discipline:** subscribe via RSS where available; don't rely on
algorithmic surfacing.

### Tier 2 — curated newsletters + podcasts (high signal, daily-weekly)

Filter-and-digest layer. Senior practitioners doing the
synthesis you'd otherwise do yourself.

- **Latent Space** (Swyx + Alessio) — newsletter + podcast.
  Cross-vendor agentic engineering. The reference channel.
- **The Pragmatic Engineer** — Gergely Orosz. Engineering-
  management framing.
- **Ben's Bites** — daily AI roundup; founder-shaped.
- **TLDR AI** — strict 3-minute brief; technical readers.
- **Import AI** — Jack Clark; policy + state-of-the-field.

**Discipline:** don't subscribe to more than 6-8. The headline
2026 stat from the
[Readless aggregator review](https://www.readless.app/blog/best-ai-newsletters-to-subscribe):
**AI publications hit 242,000 per year and growing.** Quality
filtering matters more than source quantity. Start with 8 trusted
feeds; revisit the list monthly to drop low-signal sources.

### Tier 3 — named individuals (highest signal-per-post)

The people-not-platforms layer. Specific named operators whose
output is worth following directly rather than via aggregation.
The cited 2026 set for agentic engineering:

| Person | Signal type |
|---|---|
| **Boris Cherny** (Anthropic, Head of Claude Code) | Direction-of-travel; podcast circuit; [@bcherny](https://twitter.com/bcherny) |
| **Cat Wu** (Anthropic, founding eng on Claude Code) | Engineering-side framing; UX choices |
| **Andrej Karpathy** | Broader LLM commentary; the 80% pivot post; the LLM Wiki spec |
| **Simon Willison** | Live-event coverage; deep dives on tool behaviour |
| **Swyx** | AI Engineer infrastructure; *Scaling without Slop* framing essays |
| **Cole Medin** | Long-form YouTube agent deep-dives; PIV methodology |
| **Paul Gauthier** | Aider perspective on precision tools |
| **Tom Blomfield** | YC partner; economics framing |
| **Hamel Husain** | Practitioner-engineer reflection on production ML |

**Discipline:** follow on Twitter/X via lists (not the algorithm
feed); subscribe to their personal substacks. Don't follow more
than ~15 names — beyond that the signal-to-noise drops sharply.

### Tier 4 — conference circuit (timed-event signal)

When the field gathers, the conversation crystallises. The 2026
calendar:

- **AI Engineer event series** — NYC, World's Fair, Paris, CODE,
  Europe (April 2026, see the
  [AIE Europe entry](./2026-04-aie-europe.md)). 7+ events
  planned for 2026.
- **Code w/ Claude** — Anthropic's annual conference (May
  2026 first edition, see the
  [Code Review entry](./2026-05-code-review-and-conference.md)).
- **Vendor events** — OpenAI DevDay, Google I/O, AWS re:Invent.
- **Academic** — NeurIPS, ICLR, EMNLP. Paper-shaped signal.
- **Cognition AI Ascent** — emerging.

**Discipline:** read the post-event debrief posts (Simon
Willison live blogs, Latent Space debrief episodes) rather than
trying to consume every talk individually. Conferences produce
both *signal* and *industry-PR noise*; the digest layer
separates them.

### Tier 5 — aggregators (broad, lowest signal-to-noise, useful for surface coverage)

The 2026-current aggregator picks:

- **Particle** — fast catch-up with source transparency
- **Ground News** — bias-aware across 50,000+ sources
- **Feedly** — topic monitoring; 15M+ users; the most
  developer-friendly
- **Hacker News front page** — community-curated signal across
  tech broadly
- **GitHub Trending** — language-filtered; useful for spotting
  emerging open-source patterns
- **HuggingFace Trending** — model-release surface
- **Releasebot** — automated changelog tracking across multiple
  AI vendors
- **arXiv-sanity / arXiv recent (cs.AI, cs.CL)** — paper-shaped
  signal; high noise but originality is here first

**Discipline:** **don't read aggregators daily.** Schedule a
fixed weekly skim (e.g., Sunday morning, 30 minutes). The rest
of the week, work — don't let aggregator FOMO drive the
schedule.

## The hot-topic detection rules

A topic is *hot enough to write an entry* when **at least three
of these signals fire within ~30 days**:

1. **Multiple independent writeups** — not the same essay
   reposted across three sites; *different* practitioners
   covering the topic from different angles.
2. **A senior named individual (Tier 3) talks about it** —
   Cherny / Karpathy / Swyx tweet, post, or appear on a podcast
   about it.
3. **An arxiv paper** exists or is in submission. The 2026
   ecosystem moves fast enough that papers lag practice; but
   when both happen, the topic is durable.
4. **A conference talk** is dedicated to it. Conferences are
   tier-4 timed events; their topic selection is curation.
5. **A standards body discusses it** — Linux Foundation Agentic
   AI Foundation working groups, IETF, W3C. Standardisation is
   a late-stage signal but a durable one.
6. **Multiple open-source implementations spring up** — the
   Karpathy LLM Wiki spec spawned 6+ implementations within
   months; *that's the signal*, not Karpathy publishing the
   spec alone. Implementations are votes.
7. **Anthropic / OpenAI / Google announces an integration**
   covering the topic. Vendor-side adoption is structural,
   not aspirational.

**Discipline:** when fewer than three of these fire, the topic
might be interesting personally but isn't yet *load-bearing*
across the field. Wait. Or write a brief mention in
following.md without committing to a full entry.

## The 3-source verification (anti-hallucination discipline)

Before writing an entry, verify the topic against **three
independent sources**:

1. **A primary source** — vendor docs, official announcement,
   the actual code repo
2. **A community source** — a writeup by an independent
   practitioner with citations
3. **A secondary source** — a third writeup that doesn't just
   recycle the first two

This is the discipline that prevents zeuikli-style errors (see
the [hooks entry's call-out of plausible-but-fake event
names](./2026-hooks-and-effort.md)). Specifically:

- Don't trust a single LLM-generated tutorial.
- Don't trust a single Medium post.
- Don't trust a single tweet, even from a senior name.

When all three sources agree on the load-bearing facts, the
topic is *trustworthy enough to write an entry about*.

## The cadence model

A reliable rhythm:

| Cadence | What |
|---|---|
| **Daily** | Check first-party release channels (5 min) |
| **Weekly** | Skim curated newsletters; review one or two named-individual posts (30 min) |
| **Monthly** | Audit follow-list, drop low-signal sources, add emerging ones (60 min) |
| **Per-conference** | Read the post-event debrief; decide if any topics warrant new entries (variable) |
| **Quarterly** | Re-read the existing repo entries for supersede-notes opportunities (45 min) |

The quarterly re-read is the discipline that keeps the rest of
the repo current. Every quarter, walk through the entries and
ask "what claim in this entry is now wrong?" Add supersede
notes; don't silently edit.

## The "★ earns its place through use" rule

Per [`following.md`](./following.md)'s discipline:
**a source only goes on the follow-list if a real entry cites
it.**

The reverse failure mode of underconsumption is *aspirational
consumption* — subscribing to feeds you "should" read but
don't. Aspirational consumption produces guilt + a cluttered
feed + nothing else.

The rule: when an entry cites a source, the source goes in
following.md with a ★. When following.md gets a source that
doesn't have a ★ for 6 months, drop it (move to a "former
sources" section if it was once cited but stopped producing).

The follow-list is *evidence of use*, not aspiration.

## Try this

If you don't currently have a systematic information diet for
this space:

1. **Pick one channel from each tier** — one changelog (Tier 1),
   one newsletter (Tier 2), one named individual (Tier 3), one
   conference's post-event coverage (Tier 4), one aggregator
   (Tier 5). That's five sources total. Less is more.
2. **Block 30 minutes weekly** on the calendar for the skim.
   Same time every week. Treat it as a meeting with the field.
3. **Maintain a "things to dig into" backlog** — a plain text
   file, an Obsidian note, whatever. When a topic looks
   interesting but you can't tell if it's hot yet, write it
   down with the date. Revisit weekly. Topics that survive 30
   days of mention are entry-worthy.

The point isn't to consume more. It's to consume *less, more
deliberately*. A reliable five-source diet outperforms a
chaotic 50-source one.

## Sources

- *Best AI News Aggregators in 2026: 7 Tools Compared* —
  Readless: <https://www.readless.app/blog/best-ai-news-aggregators-2026>
- *Best AI Newsletters in 2026: 12 Picks Ranked by Use Case* —
  Readless: <https://www.readless.app/blog/best-ai-newsletters-to-subscribe>
- *Best RSS Feeds for AI News in 2026* — Readless:
  <https://www.readless.app/blog/best-ai-news-rss-feeds-2026>
- *AI News Aggregator vs RSS Reader: What's the Difference?* —
  Readless:
  <https://www.readless.app/blog/ai-news-aggregator-vs-rss-reader-2026>
- *The best AI newsletters* — Zapier:
  <https://zapier.com/blog/best-ai-newsletters/>
- *The Realistic Guide to Mastering AI Agents in 2026* —
  Decoding AI: <https://www.decodingai.com/p/realistic-guide-to-ai-agents-in-2026>
- *We need to re-learn what AI agent development tools are in
  2026* — n8n:
  <https://blog.n8n.io/we-need-re-learn-what-ai-agent-development-tools-are-in-2026/>
- [`ARUNAGIRINATHAN-K/awesome-ai-agents-2026`](https://github.com/ARUNAGIRINATHAN-K/awesome-ai-agents-2026)
  — 300+ AI Agents aggregated; tier-5 surface
- [`following.md`](./following.md) in this repo — the output of
  this methodology applied to our own intake
