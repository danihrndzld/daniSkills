# My Claude Code Skills — Install Guide

A catalog of every [Claude Code](https://claude.com/claude-code) skill in my setup and the exact command to install each one.

"Skills" here actually come from **three different distribution systems** plus a couple of skills that ship built into Claude Code itself, and a few personal ones I wrote by hand. This repo exists so I (or anyone else) can rebuild the same setup from scratch.

## TL;DR — the three installers

| Ecosystem | What it is | Install one skill | Add everything from a source |
|---|---|---|---|
| **Claude Code Plugins** | Claude Code's native marketplace/plugin system. A plugin can bundle skills, agents, commands, hooks and MCP servers together. | `/plugin install <plugin>@<marketplace>` | `/plugin marketplace add <owner/repo>` then install what you want from it |
| **Skills CLI** ([`vercel-labs/skills`](https://github.com/vercel-labs/skills)) | A package manager for standalone `SKILL.md` packages, source-agnostic (GitHub, GitLab, any git URL, direct download). | `npx skills add <owner>/<repo>` | `npx skills add <owner>/<repo> --all` (or omit a `--skill` filter) |
| **Skillfish** ([`knoxgraeme/skillfish`](https://github.com/knoxgraeme/skillfish)) | A similar skill manager, syncs skills across Claude Code, Cursor, Copilot, etc. | `npx skillfish add owner/repo/path/to/skill` | `npx skillfish add owner/repo --all` |

Both `npx skills` and `npx skillfish` install to `~/.claude/skills/<name>` (global) or `.claude/skills/<name>` (per-project) — the folder just needs a `SKILL.md` with YAML frontmatter (`name`, `description`) to be picked up by Claude Code. That's the whole mechanism; everything below is really just "how do I get that folder populated."

---

## 1. Claude Code Plugins

### 1.1 Register the marketplaces

A marketplace is just a git repo Claude Code knows how to pull plugins from:

```
/plugin marketplace add anthropics/claude-plugins-official
/plugin marketplace add Yeachan-Heo/oh-my-claudecode
/plugin marketplace add EveryInc/every-marketplace
```

> The `every-marketplace` repo registers under the name `compound-engineering-plugin`, and the `oh-my-claudecode` repo registers under `omc` — that's the marketplace name you'll reference in step 1.2.

### 1.2 Install the plugins

```
/plugin install superpowers@claude-plugins-official
/plugin install frontend-design@claude-plugins-official
/plugin install code-review@claude-plugins-official
/plugin install context7@claude-plugins-official
/plugin install ralph-loop@claude-plugins-official
/plugin install security-guidance@claude-plugins-official
/plugin install claude-code-setup@claude-plugins-official
/plugin install vercel@claude-plugins-official
/plugin install chrome-devtools-mcp@claude-plugins-official
/plugin install swift-lsp@claude-plugins-official
/plugin install oh-my-claudecode@omc
/plugin install compound-engineering@compound-engineering-plugin
```

| Plugin | Marketplace | Skills it brings (namespace) |
|---|---|---|
| `superpowers` | claude-plugins-official | 14 process skills — `superpowers:brainstorming`, `systematic-debugging`, `test-driven-development`, `writing-plans`, `writing-skills`, `using-git-worktrees`, etc. |
| `frontend-design` | claude-plugins-official | `frontend-design:frontend-design` |
| `code-review` | claude-plugins-official | `code-review:code-review` + `/code-review`, `/simplify` commands |
| `context7` | claude-plugins-official | MCP server for live library docs (no standalone skill, used as a tool) |
| `ralph-loop` | claude-plugins-official | `ralph-loop:ralph-loop`, `ralph-loop:cancel-ralph`, `ralph-loop:help` |
| `security-guidance` | claude-plugins-official | `security-review` |
| `claude-code-setup` | claude-plugins-official | `claude-code-setup:claude-automation-recommender` |
| `vercel` | claude-plugins-official | ~36 skills under `vercel:*` (Next.js, AI SDK, deployments, storage, caching, auth, etc.) |
| `chrome-devtools-mcp` | claude-plugins-official | 6 skills under `chrome-devtools-mcp:*` (a11y, LCP debugging, memory leaks) |
| `swift-lsp` | claude-plugins-official | LSP tooling (no standalone skill) |
| `oh-my-claudecode` | omc | ~36 skills under `oh-my-claudecode:*` — multi-agent orchestration (`autopilot`, `ralph`, `ultrawork`, `team`, `plan`, …) |
| `compound-engineering` | compound-engineering-plugin | Workflow/agent-driven; no skill surfaced separately |

`playwright@claude-plugins-official` is also worth knowing about — I only ever installed it project-scoped (`/plugin install playwright@claude-plugins-official` run from inside a specific project), not globally.

Two more marketplaces are registered in my config but currently have **no plugin installed from them** — `pbakaus/impeccable` and `TechDufus/oh-my-claude`. The `impeccable` skills I actually use come through the Skills CLI instead (see §2).

---

## 2. Skills CLI (`npx skills`)

Install the CLI's target skill(s) straight from the source repo. Where several of my skills share one repo, one command grabs the lot.

### 2.1 `pbakaus/impeccable` — 20 design/craft skills

```
npx skills add pbakaus/impeccable
```

Installs: `adapt`, `animate`, `arrange`, `audit`, `bolder`, `clarify`, `colorize`, `critique`, `delight`, `distill`, `extract`, `frontend-design` *(a second, differently-authored `frontend-design` skill — name-collides with §1's plugin version)*, `harden`, `normalize`, `onboard`, `optimize`, `overdrive`, `polish`, `quieter`, `teach-impeccable`, `typeset`.

### 2.2 `mattpocock/skills` — engineering & productivity skills

```
npx skills add mattpocock/skills
```

Installs: `grill-me`, `grill-with-docs`, `tdd`, `improve-codebase-architecture`, `setup-matt-pocock-skills`, `handoff`.

### 2.3 `vercel-labs/skills` — skill discovery

```
npx skills add vercel-labs/skills --skill find-skills
```

Installs `find-skills` — the meta-skill that teaches Claude how to search and install more skills via this same CLI.

### 2.4 `Leonxlnx/taste-skill` — visual-design taste packs

```
npx skills add Leonxlnx/taste-skill
```

Installs: `brandkit`, `design-taste-frontend`, `design-taste-frontend-v1`, `full-output-enforcement`, `gpt-taste`, `high-end-visual-design`, `image-to-code`, `imagegen-frontend-mobile`, `imagegen-frontend-web`, `industrial-brutalist-ui`, `minimalist-ui`, `redesign-existing-projects`, `stitch-design-taste`.

### 2.5 `smithery.ai` well-known skill

```
npx skills add smithery.ai/rand/anti-slop
```

Installs `anti-slop`. The CLI resolves this through its ["well-known" discovery](https://github.com/vercel-labs/skills) mechanism rather than a git checkout — if the slug ever moves, browse [smithery.ai](https://smithery.ai/) for the current one and pass its URL directly to `npx skills add <url>`.

> Run `npx skills list` any time to see what's currently installed, and `npx skills update` to pull the latest version of everything above.

---

## 3. Skillfish (`npx skillfish`)

One skill of mine came through Skillfish instead, from a personal LaTeX-authoring repo:

```
npx skillfish add dbosk/claude-skills/latex-writing
```

Installs `latex-writing` — LaTeX/literate-programming authoring conventions (semantic environments, `csquotes`, `cleveref`, etc.).

```
skillfish list      # see everything skillfish manages
skillfish update     # pull updates
```

---

## 4. Built into Claude Code — nothing to install

A handful of skills ship with the Claude Code app itself and need no plugin or package: `dataviz`, `design`, `artifact-design`, `artifact-diagramming`, `artifact-capabilities`, `update-config`, `keybindings-help`, `fewer-permission-prompts`, `loop`, `schedule`, `claude-api`, `workflow-authoring`, `run`, `init`. They just show up once you're on a recent Claude Code build.

---

## 5. Personal / custom skills

These live only in my own `~/.claude/skills/` — they were hand-written for my own workflows (mostly Banco Industrial-specific LaTeX/deck tooling) rather than pulled from a public package, so "installing" them just means copying the folder:

| Skill | What it's for |
|---|---|
| `bc` | Generates/edits Banco Industrial "Caso de Negocio" LaTeX business cases |
| `beamer` | Beamer LaTeX slide-deck workflow (create/compile/review) |
| `htmlppt` | Self-contained static HTML presentation decks in the Banco Industrial deck style |
| `testing-and-continuous-delivery` | ISTQB test-strategy and CI/CD reference (notes in Spanish) |
| `codebase-memory` | Wraps a local `codebase-memory-mcp` MCP server for knowledge-graph queries over a codebase |
| `codebase-to-course` | Turns a codebase into an interactive single-page HTML course ([source](https://github.com/search?q=codebase-to-course), installed manually — not tracked by either CLI above) |
| `omc-reference` | Internal reference doc for the `oh-my-claudecode` plugin (`user-invocable: false`, loads automatically) |

To replicate any of these, the pattern is always the same:

```
mkdir -p ~/.claude/skills/my-skill
# write ~/.claude/skills/my-skill/SKILL.md with:
#   ---
#   name: my-skill
#   description: when Claude should reach for this
#   ---
#   ... instructions ...
```

Claude Code auto-discovers any folder under `~/.claude/skills/` (global) or `.claude/skills/` (per-project) that contains a `SKILL.md`.

---

## Notes

- Commands above assume a recent Claude Code version with the plugin/marketplace system (`/plugin`) and Node.js available for `npx`.
- Some marketplace/plugin names can shift upstream — if an `/plugin install` command 404s, run `/plugin marketplace update <name>` first, or re-add the marketplace.
- This is a snapshot of one setup, not an endorsement of every skill listed — vet anything before installing it, especially third-party skill packs (they run with the same trust as the instructions you give Claude).
