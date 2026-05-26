# 2026 — Skills, plugins, and the agentskills.io standard

## Why this matters

Skills are the **reusable instruction layer** of the three-layer stack
— the answer to "I keep telling Claude the same thing across
projects; how do I package that once?" A skill is a folder with a
`SKILL.md` (the instructions for the model) and optionally some
supporting scripts and resources. The model loads the skill into
context *when relevant* — not always, not never.

What made skills the headline 2026 ecosystem story:

1. **The plugin marketplace shipped in Claude Code itself** —
   `/plugin` is a built-in command, with the Anthropic-managed
   marketplace pre-connected.
2. **The agentskills.io open standard formalised the SKILL.md
   format** with cross-vendor portability — Claude Code, OpenAI
   Codex CLI, Cursor, Gemini CLI, GitHub Copilot all read the same
   format.
3. **The supply exploded**: from a handful of official examples at
   the start of 2026 to over a million community contributions
   indexed across marketplaces by mid-year (per the
   [tonsofskills.com aggregator](https://github.com/jeremylongshore/claude-code-plugins-plus-skills),
   numbers as of 2026-05: 425 plugins, 2,810 skills, 200 agents).

The combined effect is that a 2026 Claude Code workflow is built
*much* more around composed skills than a 2025 one was around
hand-written CLAUDE.md content.

## Skill anatomy

A skill is just a directory with a `SKILL.md` in it:

```
my-skill/
├── SKILL.md           # required — the instructions Claude reads
├── scripts/           # optional — helper scripts the skill can run
│   └── do-thing.sh
└── reference/         # optional — extra docs the skill can reference
    └── api.md
```

A minimum-viable `SKILL.md`:

```markdown
---
name: my-skill
description: One sentence on when this skill should be activated.
---

# My Skill

When the user asks for X, do Y, then Z. Use the script at
`scripts/do-thing.sh` when running step 2.

If the input matches `<some-condition>`, instead do `<other-thing>`.
```

Two things matter about the format:

- **The frontmatter `description`** is what the harness reads when
  deciding whether the skill is relevant to the current turn. Make
  it specific (when this skill applies and when it doesn't) — not
  marketing. The harness fuzzy-matches user intent against
  descriptions; vague descriptions cause spurious activation.
- **The body is loaded into context only when activated.** That's
  the whole win — you can have hundreds of skills installed without
  paying their per-session token cost upfront.

You can disable a skill without uninstalling it by renaming
`SKILL.md` to `_SKILL.md` (the harness's discovery walks for
`SKILL.md` and won't pick up the underscore-prefixed variant — see
the [Mark Chen walkthrough](https://medium.com/@markchen69/claude-code-has-a-skills-marketplace-now-a-beginner-friendly-walkthrough-8adeb67cdc89)
for the same trick documented in detail).

A subtle but consequential clarification on skill identity: **the
harness resolves skills by *folder name*; the `name:` frontmatter
is display metadata.** Folder `my-skill/` with frontmatter
`name: my-skill` is the conventional shape, but if the two diverge
the folder name wins for resolution.

## Plugins: how skills get distributed

A *plugin* is a packaging convention — one repo with one or more
skills, plus optional hooks, MCP server configs, slash commands, and
subagent definitions. A plugin's job is to bundle these into a
single installable unit.

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json    # required manifest
├── .mcp.json          # optional MCP server config
├── commands/          # optional slash commands
├── agents/            # optional subagent definitions
├── skills/            # optional — usually the bulk of the plugin
│   └── my-skill/
│       └── SKILL.md
└── hooks/             # optional hook scripts
```

The required `.claude-plugin/plugin.json` is small:

```json
{
  "name": "my-plugin",
  "version": "0.1.0",
  "description": "What this plugin does, one line.",
  "skills": "./skills"
}
```

A *marketplace* is just a repo containing a
`.claude-plugin/marketplace.json` that lists multiple plugins. The
Anthropic-managed marketplace at `anthropics/claude-plugins-official`
follows this shape; community marketplaces (netresearch's, the
tonsofskills.com one, Whittlery's eventual Genno-curated one) all do
the same thing.

## Installing — the 2026 flow

From inside any Claude Code session:

```
/plugin
```

Opens the plugin browser. Tabs: **Discover** (browse marketplaces),
**Browse** (browse installed plugins), **Manage** (uninstall /
update).

A 2026 quality-of-life addition: **the Discover and Browse screens
show what a plugin contributes before you install it** — its
commands, agents, skills, hooks, and any MCP / LSP server config.
And: **per-session projected token cost** is shown for plugin
components. So you can audit "this plugin will cost ~X tokens per
session of overhead" before committing.

The CLI equivalent:

```
claude plugin marketplace add <github-owner>/<repo>
claude plugin install <plugin-name>@<marketplace-name>
claude plugin details <plugin-name>      # the per-session cost view
```

A subtle 2026 change: **the keyboard shortcut for "remove" in the
plugin manager changed from `r` to `d`** — `r` was collidng with
"retry" and causing accidental removals. Worth knowing if your
muscle memory says `r`.

Cloning is **SSH by default** when the source is `github`, which
trips up users without a global github.com SSH key (common — many
people use per-org aliases like `github-genno`). To force HTTPS,
use the environment variable
[`CLAUDE_CODE_PLUGIN_PREFER_HTTPS`](https://www.agensi.io/learn/claude-code-plugin-marketplace-guide)
(shipped in v2.1.141):

```sh
export CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1
```

Or set up a global gitconfig rewrite:

```sh
git config --global url."https://github.com/".insteadOf "git@github.com:"
```

(See the [agensi.io marketplace
guide](https://www.agensi.io/learn/claude-code-plugin-marketplace-guide)
for the full plugin install lifecycle.)

## The agentskills.io open standard

The reason skills matter as an *ecosystem* play, not just a Claude
Code feature: the **agentskills.io** open standard formalised
`SKILL.md` as a cross-vendor format. The same skill folder runs in:

- **Claude Code** — via the plugin marketplace or direct install
- **OpenAI Codex CLI** — via `.codex-plugin/plugin.json`
- **Cursor** — via Cursor's own rules system bridging
- **Gemini CLI** — via Gemini's skill loading
- **GitHub Copilot** — via Copilot's skills surface
- **A long tail of other agents** that have adopted the standard
  ([agentskills.io](https://www.agensi.io/learn/claude-code-plugin-marketplace-guide)
  maintains the canonical list)

The wrapper manifests vary (each tool wants its own `plugin.json`
shape pointing at the skill), but the *skill itself* is portable.
For someone writing a skill that's worth multiple tools' attention,
this is a real win — you write it once, you bundle it for the tools
you care about, you don't fork the body.

## The community has converged on a hierarchy

The 2026 best-practice writeups (firecrawl, buildfastwithai,
nimbalyst, ofox, tembo) all land on the same hierarchy:

1. **Start with skills.** Easiest to author, immediate value, low
   risk. A skill is just instructions; if it's wrong, you fix the
   `SKILL.md`.
2. **Add hooks when you need deterministic enforcement.** Skills
   are advisory; hooks are mandatory. Use hooks when "the model
   complies *most* of the time" isn't good enough.
3. **Use subagents when context isolation or parallelism is worth
   the 7× token cost.** Not "feels parallel" — *structurally needs
   parallel.*

Almost every team's first three weeks of CC-platform work fall in
the same order: write a CLAUDE.md, write skills, add a hook or two,
maybe later set up a subagent.

## Try this

1. Browse the Anthropic-managed marketplace inside CC: `/plugin`,
   Discover tab. Note that *before* you install anything, the
   browser tells you what the plugin contributes (commands, hooks,
   skills, MCP servers) and an estimated per-session token cost.
2. Install one specific skill — `frontend-design` from the official
   marketplace is a good test. Restart your CC session. Ask Claude
   to design a button. Notice the skill activate (you'll see the
   skill name in the response or in tool-use traces).
3. Write your own minimal skill in a project: create
   `<project>/.claude/skills/<skill-name>/SKILL.md` with a tight
   frontmatter `description` and a one-paragraph body. Restart, ask
   Claude to do the thing the skill describes, see it activate. The
   per-project skill folder is the fastest way to feel skill
   semantics without dealing with plugin packaging.

## Sources

- [Discover and install prebuilt plugins through
  marketplaces](https://code.claude.com/docs/en/discover-plugins) —
  Anthropic Claude Code docs
- [Claude Code Plugin Marketplace Guide
  (2026)](https://www.agensi.io/learn/claude-code-plugin-marketplace-guide)
  — agensi.io
- [Organising Claude Code Skills Into Plugin Marketplaces](https://scottspence.com/posts/organising-claude-code-skills-into-plugin-marketplaces)
  — Scott Spence, 2026
- [Best Claude Code Skills to Try in
  2026](https://www.firecrawl.dev/blog/best-claude-code-skills) —
  Firecrawl
- [Claude Skills: The Complete 2026 Guide](https://www.buildfastwithai.com/blogs/claude-skills-complete-guide-2026)
  — Build Fast With AI
- [Claude Code Skills: A Practical 2026
  Guide](https://nimbalyst.com/blog/claude-code-skills-guide/) —
  Nimbalyst
- [Claude Code Has a Skills Marketplace
  Now](https://medium.com/@markchen69/claude-code-has-a-skills-marketplace-now-a-beginner-friendly-walkthrough-8adeb67cdc89)
  — Mark Chen, Medium
- [`anthropics/claude-plugins-official`](https://github.com/anthropics/claude-plugins-official)
  — the official marketplace repo
- [`netresearch/claude-code-marketplace`](https://github.com/netresearch/claude-code-marketplace)
  — community marketplace, agentskills.io reference implementation
- [`jeremylongshore/claude-code-plugins-plus-skills`](https://github.com/jeremylongshore/claude-code-plugins-plus-skills)
  — tonsofskills.com aggregator (425 plugins, 2,810 skills, 200
  agents)
