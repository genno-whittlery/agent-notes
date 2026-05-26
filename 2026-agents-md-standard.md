# 2026 (rolling) — AGENTS.md, the cross-vendor instructions standard

## Why this matters

**`AGENTS.md` is `CLAUDE.md` for every coding agent except
Claude Code.** It's a plain-Markdown standing-instructions file
that ~60,000+ GitHub repos use to guide AI coding agents —
**Codex CLI, GitHub Copilot, Cursor, Windsurf, Amp, and Devin
all read it natively.**

Originated by OpenAI in mid-2025; donated to the Linux
Foundation's **Agentic AI Foundation** in December 2025 (the
same governance move that brought
[MCP under the Foundation](./2026-mcp-protocol.md)). It's now
the de-facto cross-vendor standard for the "this is what your
agent needs to know about my project" file.

For a Claude Code user, AGENTS.md matters for two reasons:

1. **Portability.** Investment in your CLAUDE.md doesn't
   trivially port to other tools because the file's name is
   wrong everywhere else. If you ever want to try Codex CLI on
   a project, or hand off to a Cursor user, or run a Devin
   session, your instructions need to live somewhere those
   agents will read.
2. **CLAUDE.md interoperability is *pending*.** Claude Code
   today reads `CLAUDE.md` and doesn't natively read
   `AGENTS.md`. The community workaround: maintain `AGENTS.md`
   as the canonical file, symlink `CLAUDE.md` to it (or vice
   versa). Anthropic's posture on this is the question worth
   tracking.

## What AGENTS.md actually is

Per [agents.md](https://agents.md/) and the
[GitHub spec repo](https://github.com/agentsmd/agents.md):

- **A plain-Markdown file** at the project root (or in a
  subdirectory; see hierarchy below).
- **No prescribed structure.** Agents read the file as-is —
  whatever sections you write are the sections that influence
  behaviour.
- **Nearest-in-tree wins.** Agents walk the directory tree and
  read the closest `AGENTS.md`; subprojects can ship their own.

The minimum-viable file is honestly indistinguishable from a
good CLAUDE.md. Project description, build commands, code style
rules, things to do, things not to do, when-you're-stuck
guidance. The work was already done if you have a CLAUDE.md;
porting is a rename.

## The fragmentation it solved

Before AGENTS.md, every tool wanted its own instructions file:

| Tool | File it reads |
|---|---|
| Claude Code | `CLAUDE.md` |
| Cursor | `.cursorrules` |
| GitHub Copilot | `.github/copilot-instructions.md` |
| Gemini CLI | `GEMINI.md` |
| Continue.dev | `.continuerules` (some setups) |
| Codex CLI | `AGENTS.md` (since the standard's introduction) |
| Various others | their own bespoke filenames |

Developers building shareable repos were maintaining 3-5
parallel instruction files saying mostly the same thing. The
2025 OpenAI move to standardise on `AGENTS.md` was the call to
unify; the late-2025 Linux Foundation donation made it neutral
enough that competing vendors could adopt without ceding
strategic surface to a rival.

### The 2026 adoption map

| Vendor | Reads AGENTS.md natively? |
|---|---|
| OpenAI Codex CLI | Yes (it's the originator) |
| GitHub Copilot | Yes |
| Cursor | Yes |
| Windsurf | Yes |
| Amp | Yes |
| Devin / Cognition | Yes |
| **Claude Code** | **No — uses CLAUDE.md; AGENTS.md support pending** |
| **Gemini CLI** | **No — uses GEMINI.md** |
| OpenCode, Hermes Agent | Various — usually configurable |

The two outliers (Claude Code, Gemini CLI) are not coincidentally
the two vendors that already had their own established
instruction-file convention before AGENTS.md formalised. Neither
position is openly hostile — both Anthropic and Google have
indicated willingness in community channels to support AGENTS.md
eventually. The timeline is unclear.

## How to maintain both today

The 2026 community-favoured workflow for projects that want to
work with both Claude Code and AGENTS.md-reading tools:

### Option A — symlink

```sh
# AGENTS.md is the canonical file
touch AGENTS.md
ln -s AGENTS.md CLAUDE.md
```

Edit `AGENTS.md`; Claude Code reads via the symlink. Simple,
zero-duplication, works across all major filesystems.

### Option B — git-tracked dual files with shared body

Maintain both files explicitly, but with most of the content in
an `include` block or a shared `.agent-instructions.md` that
both reference. More verbose but survives operating systems
that handle symlinks badly.

### Option C — drop CLAUDE.md, use AGENTS.md, accept Claude Code
warnings

Just `AGENTS.md`. Claude Code logs that it didn't find a
CLAUDE.md; otherwise works fine because CC also reads other
context (file paths, `--add-dir`, etc.). Cheapest option if
you're OK with the log noise.

Community consensus seems to be Option A. Claude Code's
hopefully-soon native AGENTS.md support will retire the
question.

## What's in a good AGENTS.md

The same things that are in a good CLAUDE.md — see the
[CLAUDE.md + plan mode entry](./2026-claude-md-and-plan-mode.md)
for the structure. The Forrest Chang four-rule pattern works
verbatim; the minimum-viable-template in that entry's body
ports unchanged.

Two AGENTS.md-specific notes from the
[Augment Code guide](https://www.augmentcode.com/guides/how-to-build-agents-md):

- **Be explicit about commands.** Cross-tool agents have less
  built-in knowledge about your specific repo than Claude Code
  does after a single session. Spell out `npm run test` /
  `pytest` / `cargo test` even if it feels obvious.
- **Constrain scope hard.** Different agents will be tempted by
  different parts of the codebase. AGENTS.md is the single
  enforcement point for "don't touch the legacy folder; don't
  add new dependencies without asking; don't rewrite history."

## Try this

1. **If you have a CLAUDE.md you like:** copy it to AGENTS.md
   right now. `cp CLAUDE.md AGENTS.md`. Even if you only use
   Claude Code today, you've future-proofed the file for the
   tool you'll try next month.
2. **If you don't have either:** start with AGENTS.md, not
   CLAUDE.md. Symlink CLAUDE.md → AGENTS.md (Option A above).
   The canonical file lives under the cross-vendor standard.
3. **Read [the agents.md homepage](https://agents.md/)** — the
   spec is short enough to read in a couple of minutes; it
   makes concrete how lightweight the standard is.

## Sources

- [agents.md](https://agents.md/) — canonical spec homepage
- [`agentsmd/agents.md`](https://github.com/agentsmd/agents.md) —
  GitHub repo for the spec
- *Custom instructions with AGENTS.md* — OpenAI Developers
  (Codex CLI docs):
  <https://developers.openai.com/codex/guides/agents-md>
- *AGENTS.md Complete Guide 2026: Spec, Tools, Examples* —
  Codersera: <https://codersera.com/blog/agents-md-complete-guide-2026/>
- *AGENTS.md & SKILL.md: The Complete Guide (2026)* — Morph:
  <https://www.morphllm.com/agents-md-guide>
- *How to Build Your AGENTS.md (2026)* — Augment Code:
  <https://www.augmentcode.com/guides/how-to-build-agents-md>
- *AGENTS.md Patterns: What Actually Changes Agent Behavior* —
  Blake Crosley: <https://blakecrosley.com/blog/agents-md-patterns>
- Agentic AI Foundation under the Linux Foundation — December
  2025 donation destination for both AGENTS.md and MCP
