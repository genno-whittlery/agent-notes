# 2026-04 — Karpathy's LLM Wiki (filesystem-first agent memory)

## Why this matters

In April 2026, Andrej Karpathy published [a GitHub Gist titled
*LLM wiki*](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
— a **spec**, not a piece of software — articulating how to
structure a personal knowledge base so that an LLM can use it as
**long-term, durable memory beyond the context window**.

It's the most concentrated single statement of the *"context
window is the bottleneck; filesystem is the answer"* thesis that
runs through 2026 agentic-engineering practice. Karpathy's own
implementation reportedly carries **100+ articles, 400,000+ words**
of structured notes — the system isn't theoretical, he runs it as
daily AI research infrastructure.

For this notes set, it matters as **the memory-sub-discipline
canon** — the pattern most of the community implementations cite,
and the framing Cognition Labs' [*claude-memory-compiler*
post](https://www.cognitionus.com/blog/claude-memory-compiler-guide)
labels as "the first serious Karpathy-wiki implementation for
Claude Code." The seven sub-skills of agentic engineering listed
in the [umbrella entry](./2026-agentic-engineering.md) include
"memory — persistent state across sessions, usually filesystem-
backed." Karpathy's wiki is the *clearest articulation* of that
sub-skill.

## What the spec actually says

The Gist is short — single-file specification — but the load-
bearing claims:

- **Agents need durable memory beyond the context window.** Not
  longer context (which has its own tradeoffs); persistent state
  *on disk* that the agent reads in and writes back to, session
  after session.
- **Filesystem-first.** Markdown files, structured with
  wiki-style cross-links. Not a vector database. Not a custom
  RAG pipeline. Plain text the agent can read directly, files a
  human can also read.
- **Obsidian as the host environment.** Karpathy uses Obsidian
  (a Markdown-based note app) specifically because its
  wiki-linking model maps cleanly onto the structure he wants:
  pages link to other pages, the graph is queryable, the
  storage is plain `.md` files.
- **The agent's job is to maintain the wiki.** Drop new material
  in; the agent extracts concepts, creates new pages, links them
  to existing ones, keeps the graph coherent. The wiki *grows
  with use* rather than getting stale.
- **What's stored:** sessions (conversation logs), decisions
  (why we did X), procedures (how we do Y), and the network of
  linked knowledge connecting them.

## Why Obsidian specifically

Karpathy could have picked any Markdown-host. He picked Obsidian
because of three properties that align with agent-readable memory:

1. **Plain `.md` files on disk.** The agent reads them as
   ordinary text; the user reads them as rendered Markdown. No
   proprietary format, no DB lock-in, no API needed.
2. **Wiki-link syntax (`[[Other Page]]`).** Bidirectional links
   are first-class. The agent can traverse the graph; the user
   can navigate it. The same link surface serves both.
3. **Graph view and backlinks built-in.** Obsidian shows the
   structure of the wiki, which is the structure of the agent's
   memory. You can *see* where the agent is or isn't
   cross-referencing.

Other Markdown apps would work; Obsidian just removes more
friction. The spec is tool-agnostic in principle — what matters
is the **filesystem-first + wiki-linked** posture, not the
specific app.

## The community implementations

The spec spawned a community of implementations through 2026
because it's small enough to fork and large enough to be useful.
Notable ones:

| Implementation | What it does |
|---|---|
| [Cognition Labs — *claude-memory-compiler*](https://www.cognitionus.com/blog/claude-memory-compiler-guide) | First serious Karpathy-wiki implementation specifically for Claude Code. Wires Claude Code hooks + MCP server into a maintaining-the-wiki workflow. |
| [`AgriciDaniel/claude-obsidian`](https://github.com/AgriciDaniel/claude-obsidian) | Claude + Obsidian companion. `/wiki`, `/save`, `/autoresearch` slash commands. Persistent compounding vault. |
| [`kytmanov/obsidian-llm-wiki-local`](https://github.com/kytmanov/obsidian-llm-wiki-local) | 100% local via Ollama. Markdown notes → AI extracts concepts → Obsidian wiki auto-links. Zero sharing. |
| [`ar9av/obsidian-wiki`](https://github.com/ar9av/obsidian-wiki) | Framework for AI agents to build and maintain a digital brain through Obsidian wiki. Karpathy-pattern explicit. |
| [`NicholasSpisak/second-brain`](https://github.com/NicholasSpisak/second-brain) | LLM-maintained personal knowledge base for Obsidian. Karpathy-pattern explicit. |
| [`green-dalii/obsidian-llm-wiki`](https://github.com/green-dalii/obsidian-llm-wiki) | Multi-page knowledge generation with entity/concept pages + conversational query. |

The diversity of implementations is the signal: **the spec is
clear enough that ten different teams converged on similar shapes
within months of publication**. That's the marker of a durable
pattern.

## How this connects to Claude Code

The Karpathy-wiki pattern is the cleanest articulation of *how to
extend Claude Code's per-session amnesia into cross-session
durable memory*. Three integration shapes have emerged:

### Shape 1 — wiki as CLAUDE.md companion

CLAUDE.md is loaded once per session (see the
[CLAUDE.md entry](./2026-claude-md-and-plan-mode.md)). It carries
*standing instructions*. The Karpathy wiki carries *project-
specific accumulated knowledge*. The two are complementary:
CLAUDE.md tells Claude *how* to work; the wiki tells Claude *what
we've learned doing the work*. Reference the wiki from CLAUDE.md
("when context demands it, read `wiki/` for prior decisions").

### Shape 2 — wiki as agent-maintained, via hooks + MCP

Cognition's claude-memory-compiler does this: a Claude Code
`Stop` hook fires at end-of-turn, extracts new concepts from the
session, writes new pages or updates existing ones in the
Obsidian vault. An MCP server exposes the wiki as a tool surface
the agent can read on demand. The user never manually maintains
the wiki; it grows as a side effect of normal Claude Code use.

### Shape 3 — wiki as managed-agents handoff protocol

For multi-agent setups (see the
[managed-agents entry](./2026-04-managed-agents-sdk.md)): the
filesystem-with-Obsidian-shape is also the natural inter-agent
communication channel. Agent A's decisions become wiki pages;
Agent B reads them as input. The same posture as the
"shared container + isolated session" managed-agents model, just
with the *content* of the shared filesystem structured per the
Karpathy spec.

## Try this

1. **Read [the Gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).**
   It's short. Reading the source is more useful than reading
   community summaries of it.
2. **Skim one of the community implementations** —
   [`kytmanov/obsidian-llm-wiki-local`](https://github.com/kytmanov/obsidian-llm-wiki-local)
   is the simplest to start with (local Ollama, no cloud
   dependency). Note how it maps the spec to concrete code.
3. **Try the pattern on a small project** before adopting it as
   daily-driver infrastructure. The structural claim is durable;
   the *specific shape* of how you write wiki pages is best
   discovered by doing.

For Claude Code users specifically, the Cognition
*claude-memory-compiler* writeup is the right next read — it
covers the Claude-Code-side wiring (hooks + MCP) the others
don't.

## Sources

- [`karpathy/442a6bf555914893e9891c11519de94f`](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
  — Karpathy's original *LLM wiki* Gist (April 2026)
- *Andrej Karpathy's LLM Wiki: Build a Personal Knowledge Base
  with Obsidian and Codex* — MindStudio:
  <https://www.mindstudio.ai/blog/andrej-karpathy-llm-wiki-obsidian-codeex-second-brain>
- *Andrej Karpathy's LLM Wiki: Build a Self-Updating AI Second
  Brain with Obsidian in 1 Hour* — MindStudio:
  <https://www.mindstudio.ai/blog/andrej-karpathy-llm-wiki-obsidian-ai-second-brain>
- *How to Build the Knowledge System Andrej Karpathy Uses (And
  What It's Actually For)* — Tejas Sharma, Level Up Coding:
  <https://levelup.gitconnected.com/how-to-build-the-knowledge-system-andrej-karpathy-uses-and-what-its-actually-for-cf45dea0b277>
- *LLM wiki Karpathy: knowledge base with Claude and Obsidian* —
  Anthemcreation:
  <https://anthemcreation.com/en/artificial-intelligence/karpathy-llm-wiki-claude-obsidian/>
- [Cognition Labs — *claude-memory-compiler*](https://www.cognitionus.com/blog/claude-memory-compiler-guide)
  — first serious Karpathy-wiki implementation for Claude Code
- Community implementation list (above) — six independent forks
  of the spec, signal that it's a durable pattern
