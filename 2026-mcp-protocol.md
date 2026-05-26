# 2026 (rolling) — Model Context Protocol (MCP)

## Why this matters

**MCP became the cross-vendor connector standard in 2025–2026.**
Anthropic originated it; by December 2025 the protocol was
shipping **97 million monthly SDK downloads**, **10,000+ active
servers in production**, and **hundreds of distinct AI clients**
using it. In December 2025 Anthropic donated MCP to the newly
formed **Agentic AI Foundation under the Linux Foundation**,
backed by Block, OpenAI, AWS, Google, Microsoft, and others —
**MCP is no longer one company's project**.

For an agent-notes set, MCP matters because it's the protocol
under everything: when Claude Code talks to a database, a file
system, an issue tracker, a SaaS product, or another agent —
that conversation almost certainly happens over MCP. Skills
declare MCP servers they need. Hermes Agent's ACP work (see the
[Hermes entry](./2026-04-hermes-agent.md)) is partly *MCP made
agent-to-agent*. The
[Anthropic finance agents](./2026-05-claude-for-finance.md) ship
with "connectors" that are MCP servers under another name.

This entry exists because MCP was conspicuously underexplained
across the other entries — referenced in passing repeatedly,
never given its own room.

## What MCP actually is

A **client–server protocol** for connecting LLMs to external
context and tools. The pieces:

- **MCP server** — a process that exposes some capability
  (read this database, list these files, send a Slack message,
  query this API). Servers can be local processes or remote
  endpoints.
- **MCP client** — the thing using the server. Claude Code is an
  MCP client; so is Cursor, ChatGPT Desktop, Cline,
  Continue.dev, and dozens more.
- **Transport** — stdio + NDJSON (newline-delimited JSON) for
  local-process servers; HTTP+SSE or HTTP streaming for remote
  servers. The wire format is the same shape Herdr's agent
  multiplexer uses and that ACP standardises on.
- **Surface types** — tools (functions the agent can call),
  resources (data the agent can read), prompts (reusable prompt
  templates the server provides).

In practice you encounter MCP through configuration. A Claude
Code project's `.mcp.json` (or the plugin's `plugin.json`)
declares which MCP servers to spin up; Claude Code starts the
processes at session start; the servers' tools become available
to the model alongside built-in tools.

## The 2026 inflection points

### Governance — the Linux Foundation donation

In **December 2025** Anthropic donated MCP to the **Agentic AI
Foundation** (AAIF), a directed fund under the Linux Foundation.
Founding members: Block, OpenAI, AWS, Google, Microsoft,
Anthropic itself, and others. The same donation event covered
**AGENTS.md** (see that
[entry](./2026-agents-md-standard.md)).

This isn't symbolic; it changes the protocol's evolution path.
Working Groups now drive spec updates, with cross-vendor
representation. The 2026 roadmap was *reorganised around priority
areas rather than release dates* — Working/Interest Groups own
timelines for their deliverables. That's structurally different
from "Anthropic ships a new spec version when they ship one."

### Adoption — the 97M-downloads inflection

The 97M-monthly-SDK-downloads number is from December 2025
Anthropic reporting. The 10,000+ active servers in production
and hundreds of distinct AI clients are the same source. The
shape of those numbers: MCP went from *Anthropic's connector
protocol* to *the connector protocol used across the AI tooling
industry* in roughly one year.

The decisive moment for cross-vendor adoption was VS Code +
Cursor adding MCP support natively (late 2025), followed by
OpenAI's ChatGPT clients. By mid-2026 *virtually every major AI
platform or dev tool* has some level of MCP support — IDEs
(VS Code, Cursor), cloud services and databases, ChatGPT,
Claude, Gemini, the lot.

### Protocol evolution — Tasks + Server Cards

Two **2026-roadmap** additions worth naming:

#### Tasks primitive

A reliable **call-now / fetch-later** pattern for long-running
operations. Previously: an MCP tool call either returned a
result immediately or blocked the agent. Tasks let a server
acknowledge the call now and return the result later through a
separate fetch — useful for any operation that takes more than
a few seconds (model training, large data processing,
human-in-the-loop approvals).

Production use has surfaced gaps in the lifecycle semantics:
**retry semantics** (idempotency) and **expiry policies** (how
long results are retained after completion). Both are open
issues in the 2026 roadmap.

#### MCP Server Cards

A `.well-known` URL convention for exposing **structured server
metadata** *without* requiring a connection. Equivalent in
spirit to `robots.txt` or `humans.txt`: browsers, crawlers, and
registries can discover what a server does, what tools it
exposes, what auth it requires — all from a static URL. This
unblocks marketplaces / directories of MCP servers without
forcing them to actually call every server.

## What this means for Claude Code use

Three practical implications:

1. **Skills that bundle MCP servers are increasingly common.**
   See the [skills + marketplace entry](./2026-skills-and-marketplace.md)
   — the plugin format's optional `.mcp.json` is for exactly
   this. A plugin shipping a database-query MCP server alongside
   the skill that uses it is the canonical 2026 shape.
2. **Cross-vendor portability of context-access logic.** A
   well-written MCP server works in Claude Code, Cursor, ChatGPT
   Desktop, Cline, and whatever comes next. Investment in
   building good MCP servers compounds across tools the same way
   skills do.
3. **MCP Server Cards = discoverability.** Once cards are widely
   deployed, finding an MCP server for "thing I want to connect"
   becomes a search-and-install flow rather than a build-it-
   yourself flow. This is the 2026 version of "is there a
   library for that?"

## The "USB-C for AI" framing

The most cited 2026 framing of MCP — used in writeups from
[essamamdani.com](https://www.essamamdani.com/blog/complete-guide-model-context-protocol-mcp-2026)
and others — is "**USB-C for AI-native applications**." The
analogy is load-bearing: USB-C is a *physical connector* that
standardises a previously fragmented ecosystem; MCP is a
*software connector* doing the same for agent-to-tool wiring.

The analogy goes deeper than aesthetics. USB-C succeeded because
it was vendor-neutral by design (USB-IF governance), backed by
the major manufacturers, and good enough technically that
nobody could justify maintaining their own incompatible
alternative. MCP's 2025–2026 path has hit the same three
checkpoints: vendor-neutral governance (Linux Foundation),
backed by the major AI companies (Anthropic, OpenAI, Google,
Microsoft, AWS), and good enough technically that the
alternatives (function-calling-with-custom-schemas, vendor-
specific connector frameworks) have stopped seeing serious
investment.

## Try this

1. **Read the [official MCP roadmap page](https://modelcontextprotocol.io/development/roadmap).**
   Priority-area organisation, working-group structure, the
   "On the Horizon" section showing what's getting real
   community traction.
2. **Look at one MCP server's source.** The reference servers
   in `modelcontextprotocol/servers` GitHub repo are small
   enough to read end-to-end. Pick a domain you care about
   (filesystem, postgres, git) and walk through how the server
   declares its tools / resources / prompts.
3. **Install one MCP server in your Claude Code config.** The
   filesystem server is the safest place to start — exposes
   read-only file access to the model in a controlled way.
   Watch how its tools show up in CC's available-tools list.

## Sources

- [Model Context Protocol — Roadmap](https://modelcontextprotocol.io/development/roadmap)
  — official 2026 roadmap, priority-area organised
- *The 2026 MCP Roadmap* — MCP blog:
  <https://blog.modelcontextprotocol.io/posts/2026-mcp-roadmap/>
- *MCP Hits 97M Downloads: Model Context Protocol Guide* —
  Digital Applied:
  <https://www.digitalapplied.com/blog/mcp-97-million-downloads-model-context-protocol-mainstream>
- *MCP Ecosystem in 2026: What the v1.27 Release Actually Tells
  Us* — Context Studios:
  <https://www.contextstudios.ai/blog/mcp-ecosystem-in-2026-what-the-v127-release-actually-tells-us>
- *The Complete Guide to Model Context Protocol (MCP) in 2026:
  Building the USB-C for AI-Native Applications* — Essa
  Mamdani: <https://www.essamamdani.com/blog/complete-guide-model-context-protocol-mcp-2026>
- *MCP's biggest growing pains for production use will soon be
  solved* — The New Stack:
  <https://thenewstack.io/model-context-protocol-roadmap-2026/>
- *Model Context Protocol (MCP): Evolution, Capabilities, and
  the Rise of Peta* — ByteBridge / Medium:
  <https://bytebridge.medium.com/model-context-protocol-mcp-evolution-capabilities-and-the-rise-of-peta-ff2967b45d48>
- Agentic AI Foundation under the Linux Foundation — referenced
  across the writeups above as the donation destination (Dec
  2025)
