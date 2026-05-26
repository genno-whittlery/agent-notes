# 2026-05 — Claude for Finance and the 10 agent templates

## Why this matters

On **2026-05-05** — one day before *Code w/ Claude 2026* — Anthropic
shipped **Claude for Finance**: 10 ready-to-run AI agent templates
for financial services, plus a finance-tuned model release
(Claude Opus 4.7), plus Microsoft 365 add-ins, plus an enterprise
joint-venture announcement totalling $1.5B in committed capital.

For this notes set, Finance matters for the same architectural
reason that [Claude Design](./2026-04-claude-design.md) matters:
it's a **vertical surface built on the same agentic stack as
Claude Code**, with the structure made textually explicit.
[Anthropic's own announcement](https://www.anthropic.com/news/finance-agents)
defines each agent template as a *reference architecture*
packaging three things:

> 1. **Skills** — instructions and domain knowledge for the task
> 2. **Connectors** — governed access to the data the task runs on
> 3. **Subagents** — additional Claude models called by the main
>    agent for specific sub-tasks (e.g. comparables selection,
>    methodology checks)

That's exactly the agentic-engineering vocabulary the rest of this
repo runs on: skills + connectors (the finance-vertical equivalent
of MCP servers) + subagents (managed-agents-style dispatch).
**The finance launch is the cleanest worked example of "agentic
surface = skills + connectors + subagents" composing into a
production-grade vertical product.**

## The ten agents

| Agent | What it does |
|---|---|
| **Pitch builder** | Generates pitchbooks (the Wall Street deliverable) |
| **Meeting preparer** | Prepares for client meetings: collects relevant data, drafts talking points |
| **Earnings reviewer** | Reads quarterly earnings reports and surfaces what matters |
| **Model builder** | Builds financial models (DCF, etc.) |
| **Market researcher** | Synthesises market intelligence |
| **Valuation reviewer** | Reviews valuation work |
| **General ledger reconciler** | GL reconciliation (back-office accounting) |
| **Month-end closer** | Month-end close work |
| **Statement auditor** | Reviews financial statements |
| **KYC screener** | Know-Your-Customer screening |

Spread across both **front-office** (pitch builder, meeting
preparer, market researcher) and **back-office** (GL reconciler,
month-end closer, statement auditor, KYC screener) — Anthropic
explicitly targeted both sides of the finance workforce, not just
the high-billable analyst-shaped roles.

What's worth noting from the [Tort Mario writeup](https://medium.com/@tort_mario/anthropics-new-ai-agents-for-finance-what-they-do-why-they-matter-and-how-they-actually-work-5379502317fd):
these aren't *demo agents*. They ship as templates customers can
fork, customise to their firm's data + conventions, and run as
production tooling. The market shape is **template-and-customise**,
not **out-of-the-box-product**.

## The Opus 4.7 finance benchmark

Anthropic released **Claude Opus 4.7** alongside the finance
launch and pinned its identity to a benchmark number: **64.37% on
Vals AI's Finance Agent benchmark**, leading the industry. Two
useful framings of that number:

- **It's a finance-specific eval, not general.** Vals AI's
  Finance Agent benchmark is what financial-services buyers care
  about; it's the right number for *this* vertical, not necessarily
  a sign that Opus 4.7 is broadly best-in-class. (Though for
  Claude Code users: Opus 4.7 is the same model backing CC at the
  Opus tier.)
- **A 64% score on a vertical benchmark is not a "solved" number.**
  The agent gets ~2 out of 3 tasks right. The finance launch is
  positioned as *accelerator for analysts*, not *replacement for
  analysts* — the numbers force that framing whether the marketing
  copy emphasises it or not.

## Microsoft 365 integration

The other piece that pulls Claude further out of "chat app"
territory: **Claude now works across Microsoft Excel, PowerPoint,
Word, and Outlook through Microsoft 365 add-ins.** Once installed,
**context moves between applications automatically** — work that
starts in a spreadsheet continues in a deck without the analyst
re-explaining.

The cross-app context propagation is the architectural piece. It
matches the same continuity-of-context shape we saw in the
[Claude Design entry](./2026-04-claude-design.md) (design system
read once, applied to every project) and the
[CLAUDE.md entry](./2026-claude-md-and-plan-mode.md) (project
context loaded once per session). **Persistent context across
surfaces is the 2026 Anthropic motif** — chat, code, design, and
now finance all share the pattern.

## The $1.5B enterprise joint venture

Per [Fortune's coverage](https://fortune.com/2026/05/05/anthropic-wall-street-financial-services-agents-jamie-dimon/):

- **$300M** each from Anthropic, Blackstone, and Hellman &
  Friedman
- **$150M** from Goldman Sachs
- Apollo Global Management, General Atlantic, Leonard Green, GIC,
  Sequoia Capital also participated
- Total: **~$1.5B** for enterprise services

This is the *bet under the bet*. The finance agent templates are a
product launch; the $1.5B JV is the **distribution commitment**
backing it. Goldman, Blackstone, and H&F aren't just buying — they
have skin in selling enterprise-grade Claude integrations into
their portfolio companies.

For someone working with Claude Code, this matters indirectly:
the platform's commercial trajectory determines the platform's
investment trajectory. Anthropic that's $1.5B-JV-shaped invests
differently than Anthropic that's only-API-revenue-shaped.

## Connection to Claude Code

The finance vertical pulls into this Claude-Code-adjacent notes
set because *the agent templates are constructed from the same
primitives Claude Code exposes*. If you:

- Read Anthropic's finance-agent reference architectures
- Compared them to your own Claude Code workflows
- Asked "what would my project look like packaged this way?"

…you'd find your workflow already has the same pieces: skills
(reusable instructions for recurring tasks), connectors (MCP
servers / `--add-dir` / file references), subagents (Task tool
dispatches for context-isolation). The finance agents are *your
workflow, formalised as deliverables.* That's the thing worth
noticing.

## Try this

1. **Read [Anthropic's finance-agents
   announcement](https://www.anthropic.com/news/finance-agents)
   for the architecture diagram.** The skills/connectors/subagents
   decomposition is the cleanest single statement of the agentic-
   surface shape Anthropic is building toward across all
   verticals.
2. **Pick one of the 10 agent templates.** Try to write the same
   thing as a Claude Code skill + plugin in your own project.
   You won't ship a production-grade KYC screener this way (no
   regulated-data connectors), but you'll feel the gap between
   "skill that helps a task" and "agent that runs a workflow."
3. **Read the [Vals AI Finance Agent
   benchmark](https://vals.ai/)** if you care about evaluation
   discipline. Vertical-specific evals are how this generation
   of agentic products will get scored; understanding the eval
   shape is part of agentic-engineering literacy.

## Sources

- *Agents for financial services* — Anthropic (2026-05-05 primary
  announcement, with the skills/connectors/subagents architecture
  reference): <https://www.anthropic.com/news/finance-agents>
- *Anthropic deepens push into Wall Street with new AI agents,
  full Microsoft 365 integration, Moody's data partnership* —
  Fortune (2026-05-05):
  <https://fortune.com/2026/05/05/anthropic-wall-street-financial-services-agents-jamie-dimon/>
- *Anthropic unleashes finance agents for Claude* — The Register:
  <https://www.theregister.com/2026/05/05/anthropic_unleashes_finance_agents_claude/>
- *Claude for Finance: Anthropic's 10 AI Agents Hit Wall Street* —
  Pasquale Pillitteri:
  <https://pasqualepillitteri.it/en/news/2173/claude-finance-anthropic-10-ai-agents-2026>
- *Anthropic's New AI Agents for Finance: What They Do, Why They
  Matter, and How They Actually Work* — Tort Mario, Medium
  (deepest single architecture writeup):
  <https://medium.com/@tort_mario/anthropics-new-ai-agents-for-finance-what-they-do-why-they-matter-and-how-they-actually-work-5379502317fd>
- *Anthropic Launches Ten Finance Agent Templates for Claude* —
  Let's Data Science:
  <https://letsdatascience.com/news/anthropic-launches-ten-finance-agent-templates-for-claude-6516f048>
- *Anthropic's Explosive Start to 2026: Everything Claude Has
  Launched* — Fazal, Medium (the broader release-cadence context):
  <https://fazal-sec.medium.com/anthropics-explosive-start-to-2026-everything-claude-has-launched-and-why-it-s-shaking-up-the-668788c2c9de>
