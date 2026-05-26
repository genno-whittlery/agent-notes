# 2026 (rolling) — AI browsers: ChatGPT Atlas, Perplexity Comet, and the agentic-browser category

## Why this matters

In late 2025, two AI-first browsers shipped within months of each
other: **Perplexity Comet** (July 2025) and **ChatGPT Atlas**
(October 21, 2025). By 2026 the category had a name —
**agentic browsers** — and a defining feature: **the browser is
no longer a tool the agent uses, the agent is a tool the
browser is built around.**

This entry exists because the agentic-browser category is
structurally adjacent to Claude Code in a specific way: it's
**the same "agent that drives a controlled environment"
pattern**, applied to the web instead of to a codebase. The
parallels with [Webwright](https://github.com/microsoft/Webwright)
(Microsoft Research's CC-as-browser-agent skill, see the
plugin context across the [skills entry](./2026-skills-and-marketplace.md))
are explicit. The agent-controlling-a-browser shape is one of
the highest-leverage shapes in 2026, period.

## The two main agents

### ChatGPT Atlas

Launched **2025-10-21** by OpenAI. Chromium-based browser with
ChatGPT integrated at its core. Initially macOS-only; in
**March 2026**, OpenAI announced Atlas would be **merged with
ChatGPT and Codex into a single desktop superapp** — Windows
release timeline unclear.

Architecturally a Chrome fork (familiar UI, tab model,
extensions) with ChatGPT as the *default search engine* and as
an *omnipresent agent* available on every tab. Agent
functionality requires ChatGPT Plus ($20/mo) or higher.

The headline use-case Atlas pitches: **autonomous web research**.
Open a tab, give Atlas a task ("compare these five SaaS vendors
on pricing, integrations, customer reviews"), watch it navigate
through tabs, take notes, surface conclusions. The same shape as
a human research workflow, autonomously.

### Perplexity Comet

Launched **2025-07** by Perplexity. Same agentic-browser
category. Comet went from **$200/month exclusivity** to
**completely free** in October 2025 — when Perplexity removed
the waitlist and made it accessible to all users at no cost.
The pricing move is itself the story: when an AI product flips
from $200/mo to free, the platform play is the strategic move,
not the per-user revenue.

Comet's specific pitch: **proactive AI assistants that guide
users toward outcomes rather than links.** Designed as
AI-first; replaces traditional search-driven interactions
entirely.

## What "agentic browser" means as a category

Three properties define the category, per the 2026 comparison
writeups:

1. **The agent is always available.** Not a "click this button
   to summon AI" surface; the AI is a persistent sidebar / chat
   / floating action that follows the user across tabs.
2. **The agent can act on the browser.** Click links, fill
   forms, navigate, scroll, copy content. This is the
   structurally different bit from "ChatGPT in a browser tab"
   — the agent isn't *inside the page*, it's *driving the
   browser*.
3. **Cross-tab context.** What you read on one tab informs
   what the agent does on the next. The browser becomes a
   *state surface* the agent uses, similar to how a filesystem
   is a state surface for Claude Code.

The adjacent technology stack: **Claude Computer Use** (the
Anthropic API capability that lets a model click and type
through a desktop interface) is the underlying primitive that
makes this category technically feasible. Atlas, Comet, and
similar tools wrap this primitive into a product surface.

## The Webwright comparison

[Microsoft Research's Webwright](https://github.com/microsoft/Webwright)
is the *DIY* version of the same idea — a Playwright workspace
the agent (Claude Code, Codex, OpenCode, Hermes, Pi) drives one
bash command at a time. The
[Pinggy guide to AI harnesses](https://pinggy.io/blog/best_ai_harnesses_to_supercharge_llm_models/)
framing applies here: Atlas / Comet are *productised* agentic
browser harnesses; Webwright is the *DIY* harness for the same
work.

For someone who'd otherwise spend $20/mo on Atlas or use Comet
free: Webwright + Claude Code gives you the same agentic-
browser capability *inside your own agent harness*, with full
control. The tradeoff: Webwright runs Playwright (a real
browser engine) under your harness; Atlas / Comet run their
own UX-tuned product. Different shape, same primitive.

## What this means for Claude Code use

Three durable claims:

1. **Browser-driving agency is a 2026-mainstream pattern.** Up
   until late 2025 this was research / demo territory; from Q4
   2025 onward it's a product category with multiple players.
   The technical primitive (Claude Computer Use, OpenAI's
   equivalent) is mature enough that productisation can compete
   on UX, not on whether-it-works.
2. **The browser is becoming an "MCP server" in shape.** Per
   the [MCP entry](./2026-mcp-protocol.md): MCP servers expose
   tools the agent can call. An agentic-browser product is the
   browser-as-a-tool-surface — the agent's primary verb is
   "navigate to a URL and do something." This is a category-
   level convergence: agents, regardless of their wrapping,
   end up driving similar primitives (filesystem, browser,
   shell).
3. **The merger announcement matters.** OpenAI's March 2026
   announcement of merging Atlas + ChatGPT + Codex into a
   single desktop superapp is the *commercial* version of what
   Anthropic shipped with managed agents — different surfaces
   (browser, chat, CLI) become different *views* of the same
   agent infrastructure. Expect Anthropic-side equivalents to
   follow.

## How the AI browsers compare

| Browser | Released | Free? | Best for |
|---|---|---|---|
| **Perplexity Comet** | July 2025 | Yes (since Oct 2025) | Perplexity users; research-heavy work |
| **ChatGPT Atlas** | Oct 2025 | $20/mo for agent features | ChatGPT users; mac-only as of Feb 2026 |
| **Arc Browser** (legacy) | Pre-2025 | Yes | Now in maintenance mode; the 'AI features' were retrofit |
| **Dia** (The Browser Company) | 2025 | Beta access | Arc's successor; AI-first design |

The 2026 community consensus: **Atlas and Comet are great for
their respective ecosystems**; pick based on whether you use
ChatGPT or Perplexity as your primary chatbot. The category
itself is moving faster than any browser cycle since Chrome
launched — *expect feature parity between leaders to shift
quarterly.*

## Try this

If you spend any meaningful time on browser-based research:

1. **Try Comet for a week.** It's free; no commitment beyond
   downloading. Pick a research task you'd normally do across
   30+ tabs. Watch what Comet does differently.
2. **Compare against Webwright + Claude Code** doing the same
   task. The browser-agent shape is the same; the harness
   wrapping is different. Which one fits your workflow better
   says something about whether you want browser-as-product
   or browser-as-tool.
3. **Read [the AI browser landscape
   overview](https://www.digitalapplied.com/blog/ai-browser-landscape-2026-atlas-comet-arc-dia)**
   for the broader context — Arc, Dia, and the pre-AI-browser
   wave that's now retrofitting.

The point isn't to switch browsers. It's to recognise that
**"agent drives a controlled environment" is a 2026 shape
that productises across many surfaces** — codebases (Claude
Code), web (Atlas, Comet), 3D modelling (Claude Design's
Autodesk connector), audio (Ableton connector), finance
workflows (Anthropic finance agents). The harness changes; the
underlying pattern is the same.

## Sources

- *ChatGPT Atlas vs Comet Browser: Which Should You Choose?* —
  Efficient App: <https://efficient.app/compare/atlas-vs-comet>
- *ChatGPT Atlas vs Perplexity Comet: How Agentic Browsers
  Work* — HumanSecurity:
  <https://www.humansecurity.com/learn/blog/chatgpt-atlas-vs-perplexity-comet-agentic-browsers/>
- *AI Browser Landscape 2026: Atlas vs Comet vs Arc vs Dia* —
  Digital Applied:
  <https://www.digitalapplied.com/blog/ai-browser-landscape-2026-atlas-comet-arc-dia>
- *AI Browsers: The Rise of ChatGPT Atlas, Perplexity Comet,
  and the Next Generation of Web Exploration* — News Nest
  (2026-02-05):
  <https://news-nest.com/2026/02/05/ai-browsers-the-rise-of-chatgpt-atlas-perplexity-comet-and-the-next-generation-of-web-exploration/>
- *ChatGPT Atlas: OpenAI AI Browser Strategy Guide 2026* —
  Digital Applied:
  <https://www.digitalapplied.com/blog/chatgpt-atlas-openai-ai-browser-strategy-guide>
- *The Agentic Browser Landscape in 2026: A Complete Guide* —
  No Hacks: <https://nohacks.co/blog/agentic-browser-landscape-2026>
- *How Agentic Browsers Are Changing SEO* — SUSO Digital:
  <https://susodigital.com/thoughts/what-agentic-browsers-mean-for-seo/>
