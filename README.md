# My Claude Code Skills — Install Guide

A catalog of every [Claude Code](https://claude.com/claude-code) skill in my setup and the exact command to install each one — ordered by how central each is to my day-to-day workflow: **OMC → Compound Engineering → Matt Pocock skills → Anti-slop → everything else.**

"Skills" here come from three different distribution systems, plus a couple that ship built into Claude Code itself and a few I wrote by hand. For every entry below you get two install paths:

- **Normal way** — the one-liner (`/plugin install`, `npx skills add`, `npx skillfish add`). Needs Node.js for the `npx` ones.
- **Manual (no Node.js)** — clone/download the raw files yourself and drop them in `~/.claude/skills/<name>/`. **PowerShell first, macOS/Linux (bash/zsh) second.** Use this on a machine where you can't or won't install Node.

Either path produces the same thing: a folder containing a `SKILL.md` (YAML frontmatter with `name` + `description`, then instructions) under `~/.claude/skills/` (global) or `.claude/skills/` (per-project). That's the entire discovery mechanism — Claude Code just walks those folders.

## Quickstart — bootstrap prompt for a new machine

Don't want to read the tables below? Paste this whole block into a fresh Claude Code session on any machine (Windows, macOS, or Linux). It checks what's available, installs what it can, and then installs sections 1–3 (OMC, Compound Engineering, all six Matt Pocock skills) using whichever method actually works on that machine.

```
You're bootstrapping a new machine with a specific set of Claude Code skills from
https://github.com/danihrndzld/daniSkills — follow these steps in order.

STEP 1 — Check tools.
Determine whether `git` is on PATH, and whether `node`/`npx` are on PATH. Report the
version of each if found.

STEP 2 — Install what's missing, if you safely can.
If `git` is missing, try the best method for this OS: `winget install --id Git.Git -e`
on Windows, `xcode-select --install` (or `brew install git` if Homebrew exists) on
macOS, `apt-get install -y git` / `dnf install -y git` / `pacman -S git` on Linux.
If `node`/`npx` are missing, try `winget install OpenJS.NodeJS.LTS` on Windows,
`brew install node` on macOS, or the OS package manager / nvm on Linux.
Ask me for confirmation before anything that needs admin/sudo privileges. If an
install fails or isn't possible, move on rather than getting stuck — this is best
effort, not required.

STEP 3 — Report.
Tell me plainly what's usable right now (git: yes/no + version, node/npx: yes/no +
version) and what you attempted, and whether each attempt succeeded or failed.

STEP 4 — Install the skills.
Fetch https://github.com/danihrndzld/daniSkills (clone it, or read the README
directly) and follow ONLY sections 1, 2, and 3 of its README:
  - Section 1: oh-my-claudecode (OMC)
  - Section 2: Compound Engineering
  - Section 3: Matt Pocock skills — install all six (grill-me, grill-with-docs, tdd,
    improve-codebase-architecture, setup-matt-pocock-skills, handoff)
Do not touch section 4 onward.
For each section, use its "Normal way" commands if `/plugin` and/or `node`/`npx` are
available; otherwise use its "Manual (no Node.js)" commands, picking the PowerShell
block on Windows or the macOS/Linux block everywhere else — match whichever this
machine's tool-check in Step 1/2 actually landed on.

STEP 5 — Confirm.
List exactly which skills ended up installed (and where — `~/.claude/skills/<name>`
or via `/plugin`), and flag anything you had to skip and why.
```

## TL;DR — the three installers

| Ecosystem | What it is | Normal install |
|---|---|---|
| **Claude Code Plugins** | Native marketplace/plugin system. A plugin can bundle skills + agents + commands + hooks + MCP servers. | `/plugin marketplace add <owner/repo>` then `/plugin install <plugin>@<marketplace>` |
| **Skills CLI** ([`vercel-labs/skills`](https://github.com/vercel-labs/skills)) | Package manager for standalone `SKILL.md` packages — GitHub, GitLab, any git URL, direct download. | `npx skills add <owner>/<repo>` |
| **Skillfish** ([`knoxgraeme/skillfish`](https://github.com/knoxgraeme/skillfish)) | Similar skill manager, syncs across Claude Code, Cursor, Copilot, etc. | `npx skillfish add owner/repo/path` |

---

## 1. oh-my-claudecode (OMC)

Multi-agent orchestration layer — 38 skills (`autopilot`, `ralph`, `ultrawork`, `team`, `plan`, `debug`, `wiki`, `remember`, …) plus its own agents, hooks and MCP tools.

**Normal way:**

```
/plugin marketplace add Yeachan-Heo/oh-my-claudecode
/plugin install oh-my-claudecode@omc
```

**Manual (no Node.js)** — the skills live at repo root under `skills/`:

PowerShell:
```powershell
git clone --depth 1 https://github.com/Yeachan-Heo/oh-my-claudecode.git $env:TEMP\oh-my-claudecode
Copy-Item -Recurse -Force "$env:TEMP\oh-my-claudecode\skills\*" "$env:USERPROFILE\.claude\skills\"
```

macOS/Linux:
```bash
git clone --depth 1 https://github.com/Yeachan-Heo/oh-my-claudecode.git /tmp/oh-my-claudecode
cp -R /tmp/oh-my-claudecode/skills/. ~/.claude/skills/
```

> Manual copy gives you the skills only — you lose the plugin's bundled agents, hooks and MCP servers, which do need `/plugin install`.

---

## 2. Compound Engineering

38 `ce-*` skills (`ce-work`, `ce-plan`, `ce-brainstorm`, `ce-code-review`, `ce-worktree`, …) plus ~50 review-specialist agents, from Every's marketplace repo.

**Normal way:**

```
/plugin marketplace add EveryInc/every-marketplace
/plugin install compound-engineering@compound-engineering-plugin
```

**Manual (no Node.js)** — the plugin lives at `plugins/compound-engineering/skills/` inside the marketplace repo, so a sparse checkout avoids pulling the whole thing:

PowerShell:
```powershell
git clone --depth 1 --filter=blob:none --sparse https://github.com/EveryInc/every-marketplace.git $env:TEMP\every-marketplace
Push-Location $env:TEMP\every-marketplace
git sparse-checkout set plugins/compound-engineering
Pop-Location
Copy-Item -Recurse -Force "$env:TEMP\every-marketplace\plugins\compound-engineering\skills\*" "$env:USERPROFILE\.claude\skills\"
```

macOS/Linux:
```bash
git clone --depth 1 --filter=blob:none --sparse https://github.com/EveryInc/every-marketplace.git /tmp/every-marketplace
git -C /tmp/every-marketplace sparse-checkout set plugins/compound-engineering
cp -R /tmp/every-marketplace/plugins/compound-engineering/skills/. ~/.claude/skills/
```

> The same marketplace also hosts a `coding-tutor` plugin (`plugins/coding-tutor`) I haven't installed — same recipe, different path, if you want it.
> Note: even with the plugin enabled, the `ce-*` skills haven't been surfacing as invocable in my sessions — if you hit the same thing, the manual copy above is a reliable workaround.

---

## 3. Matt Pocock skills (`mattpocock/skills`)

Engineering/productivity skills: `grill-me`, `grill-with-docs`, `tdd`, `improve-codebase-architecture`, `setup-matt-pocock-skills`, `handoff`.

**Normal way:**

```
npx skills add mattpocock/skills
```

**Manual (no Node.js):**

PowerShell:
```powershell
git clone --depth 1 https://github.com/mattpocock/skills.git $env:TEMP\mattpocock-skills
$src = "$env:TEMP\mattpocock-skills\skills"
$dst = "$env:USERPROFILE\.claude\skills"
New-Item -ItemType Directory -Force -Path "$dst\grill-me", "$dst\grill-with-docs", "$dst\tdd", "$dst\improve-codebase-architecture", "$dst\setup-matt-pocock-skills", "$dst\handoff" | Out-Null
Copy-Item -Recurse -Force "$src\productivity\grill-me\*"                      "$dst\grill-me"
Copy-Item -Recurse -Force "$src\engineering\grill-with-docs\*"                "$dst\grill-with-docs"
Copy-Item -Recurse -Force "$src\engineering\tdd\*"                            "$dst\tdd"
Copy-Item -Recurse -Force "$src\engineering\improve-codebase-architecture\*"  "$dst\improve-codebase-architecture"
Copy-Item -Recurse -Force "$src\engineering\setup-matt-pocock-skills\*"       "$dst\setup-matt-pocock-skills"
Copy-Item -Recurse -Force "$src\productivity\handoff\*"                      "$dst\handoff"
```

macOS/Linux:
```bash
git clone --depth 1 https://github.com/mattpocock/skills.git /tmp/mattpocock-skills
src=/tmp/mattpocock-skills/skills
dst=~/.claude/skills
mkdir -p "$dst"/{grill-me,grill-with-docs,tdd,improve-codebase-architecture,setup-matt-pocock-skills,handoff}
cp -R "$src/productivity/grill-me/."                     "$dst/grill-me/"
cp -R "$src/engineering/grill-with-docs/."                "$dst/grill-with-docs/"
cp -R "$src/engineering/tdd/."                            "$dst/tdd/"
cp -R "$src/engineering/improve-codebase-architecture/."  "$dst/improve-codebase-architecture/"
cp -R "$src/engineering/setup-matt-pocock-skills/."       "$dst/setup-matt-pocock-skills/"
cp -R "$src/productivity/handoff/."                       "$dst/handoff/"
```

---

## 4. Anti-slop

Toolkit for detecting/removing generic "AI slop" patterns in text, code and design — `SKILL.md`, 3 reference guides, 2 Python scripts.

**Normal way:**

```
npx skills add smithery.ai/rand/anti-slop
```

**Manual (no Node.js):** this one resolves through the Skills CLI's "well-known" discovery rather than a git repo, so there's no repo to clone. You can pull the core `SKILL.md` directly:

PowerShell:
```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills\anti-slop" | Out-Null
Invoke-WebRequest -Uri "https://smithery.ai/skills/rand/anti-slop/.well-known/skills/anti-slop/SKILL.md" -OutFile "$env:USERPROFILE\.claude\skills\anti-slop\SKILL.md"
```

macOS/Linux:
```bash
mkdir -p ~/.claude/skills/anti-slop
curl -fsSL "https://smithery.ai/skills/rand/anti-slop/.well-known/skills/anti-slop/SKILL.md" -o ~/.claude/skills/anti-slop/SKILL.md
```

> That gets you the skill itself. The bundled reference guides (`references/*.md`) and helper scripts (`scripts/*.py`) it points to are optional extras described inside `SKILL.md` — the skill works fully without them; re-add them individually if you want the automated detectors too.

---

## 5. Everything else

### 5.1 Skills CLI — more sources

**`pbakaus/impeccable`** — 20 design/craft skills (`adapt`, `animate`, `arrange`, `audit`, `bolder`, `clarify`, `colorize`, `critique`, `delight`, `distill`, `extract`, a second `frontend-design`, `harden`, `normalize`, `onboard`, `optimize`, `overdrive`, `polish`, `quieter`, `teach-impeccable`, `typeset`):

```
npx skills add pbakaus/impeccable
```

Manual — PowerShell:
```powershell
git clone --depth 1 https://github.com/pbakaus/impeccable.git $env:TEMP\impeccable
Copy-Item -Recurse -Force "$env:TEMP\impeccable\.claude\skills\*" "$env:USERPROFILE\.claude\skills\"
```
Manual — macOS/Linux:
```bash
git clone --depth 1 https://github.com/pbakaus/impeccable.git /tmp/impeccable
cp -R /tmp/impeccable/.claude/skills/. ~/.claude/skills/
```

**`vercel-labs/skills`** — `find-skills`, the meta-skill that teaches Claude to search/install more skills via this CLI:

```
npx skills add vercel-labs/skills --skill find-skills
```

Manual — PowerShell:
```powershell
git clone --depth 1 https://github.com/vercel-labs/skills.git $env:TEMP\vercel-labs-skills
Copy-Item -Recurse -Force "$env:TEMP\vercel-labs-skills\skills\find-skills" "$env:USERPROFILE\.claude\skills\find-skills"
```
Manual — macOS/Linux:
```bash
git clone --depth 1 https://github.com/vercel-labs/skills.git /tmp/vercel-labs-skills
mkdir -p ~/.claude/skills/find-skills
cp -R /tmp/vercel-labs-skills/skills/find-skills/. ~/.claude/skills/find-skills/
```

**`Leonxlnx/taste-skill`** — 13 visual-design taste packs (`brandkit`, `design-taste-frontend`(-v1), `full-output-enforcement`, `gpt-taste`, `high-end-visual-design`, `image-to-code`, `imagegen-frontend-mobile`, `imagegen-frontend-web`, `industrial-brutalist-ui`, `minimalist-ui`, `redesign-existing-projects`, `stitch-design-taste`):

```
npx skills add Leonxlnx/taste-skill
```

Manual — clone `https://github.com/Leonxlnx/taste-skill.git` and copy from its `skills/` folder the same way as above (each package lives under its own subfolder, e.g. `skills/brandkit`, `skills/taste-skill`, `skills/output-skill`, etc. — run `npx skills list` after the CLI install once to see the exact name↔folder mapping if you need it).

### 5.2 Skillfish — `latex-writing`

LaTeX/literate-programming authoring conventions, from a personal repo:

```
npx skillfish add dbosk/claude-skills/latex-writing
```

Manual — PowerShell:
```powershell
git clone --depth 1 https://github.com/dbosk/claude-skills.git $env:TEMP\claude-skills
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills\latex-writing" | Out-Null
Copy-Item -Recurse -Force "$env:TEMP\claude-skills\latex-writing\*" "$env:USERPROFILE\.claude\skills\latex-writing"
```
Manual — macOS/Linux:
```bash
git clone --depth 1 https://github.com/dbosk/claude-skills.git /tmp/claude-skills
mkdir -p ~/.claude/skills/latex-writing
cp -R /tmp/claude-skills/latex-writing/. ~/.claude/skills/latex-writing/
```

### 5.3 More Claude Code Plugins that expose skills

All five below install from the same official marketplace — you only need to add it once:

```
/plugin marketplace add anthropics/claude-plugins-official
/plugin install superpowers@claude-plugins-official
/plugin install frontend-design@claude-plugins-official
/plugin install claude-code-setup@claude-plugins-official
/plugin install vercel@claude-plugins-official
/plugin install chrome-devtools-mcp@claude-plugins-official
```

| Plugin | Skills | Actual upstream source (for manual installs) |
|---|---|---|
| `superpowers` | 14 process skills — `brainstorming`, `systematic-debugging`, `test-driven-development`, `writing-plans`, `writing-skills`, `using-git-worktrees`, etc. | `obra/superpowers` (root `skills/`) |
| `frontend-design` | `frontend-design:frontend-design` | inside `anthropics/claude-plugins-official` at `plugins/frontend-design/skills/` |
| `claude-code-setup` | `claude-code-setup:claude-automation-recommender` | same repo, `plugins/claude-code-setup/skills/` |
| `vercel` | 33 skills under `vercel:*` (Next.js, AI SDK, deployments, storage, caching, auth, …) | `vercel/vercel-plugin` (root `skills/`) |
| `chrome-devtools-mcp` | 6 skills — `a11y-debugging`, `chrome-devtools`, `chrome-devtools-cli`, `debug-optimize-lcp`, `memory-leak-debugging`, `troubleshooting` | `ChromeDevTools/chrome-devtools-mcp` (root `skills/`) |

The official marketplace is just an aggregator — it points at those upstream repos internally. So the manual (no Node.js) fallback targets the upstream repo, not `claude-plugins-official` itself: `git clone` it directly for `superpowers`, `vercel`, and `chrome-devtools-mcp` (their `skills/` folder is at repo root, same recipe as §1); sparse-checkout `plugins/frontend-design` or `plugins/claude-code-setup` from `anthropics/claude-plugins-official` for those two (same recipe as §2's sparse-checkout, just a different path).

### 5.4 Plugins with no standalone skill files

`code-review`, `ralph-loop`, `security-guidance`, `swift-lsp`, `context7` and `playwright` (all from `claude-plugins-official`) are implemented as slash commands, hooks, LSP config or MCP servers rather than `SKILL.md` packages — there's no text file to copy manually. `/plugin install <name>@claude-plugins-official` is the only install path for these.

### 5.5 Built into Claude Code — nothing to install

`dataviz`, `design`, `artifact-design`, `artifact-diagramming`, `artifact-capabilities`, `update-config`, `keybindings-help`, `fewer-permission-prompts`, `loop`, `schedule`, `claude-api`, `workflow-authoring`, `run`, `init` ship with the Claude Code app itself — they just show up on a recent build, nothing to install.

### 5.6 Personal / custom skills

Hand-written for my own workflows (mostly Banco Industrial-specific LaTeX/deck tooling) — no public package, so "installing" one just means creating the folder yourself:

| Skill | What it's for |
|---|---|
| `bc` | Generates/edits Banco Industrial "Caso de Negocio" LaTeX business cases |
| `beamer` | Beamer LaTeX slide-deck workflow (create/compile/review) |
| `htmlppt` | Self-contained static HTML presentation decks in the Banco Industrial deck style |
| `testing-and-continuous-delivery` | ISTQB test-strategy and CI/CD reference (notes in Spanish) |
| `codebase-memory` | Wraps a local `codebase-memory-mcp` MCP server for knowledge-graph queries over a codebase |
| `codebase-to-course` | Turns a codebase into an interactive single-page HTML course (installed manually, not tracked by either CLI above) |
| `omc-reference` | Internal reference doc for the `oh-my-claudecode` plugin (`user-invocable: false`, loads automatically) |

The pattern for any of these, on either OS:

PowerShell:
```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills\my-skill"
# then create $env:USERPROFILE\.claude\skills\my-skill\SKILL.md with:
#   ---
#   name: my-skill
#   description: when Claude should reach for this
#   ---
#   ... instructions ...
```
macOS/Linux:
```bash
mkdir -p ~/.claude/skills/my-skill
# then create ~/.claude/skills/my-skill/SKILL.md with the same frontmatter
```

Claude Code auto-discovers any folder under `~/.claude/skills/` (global) or `.claude/skills/` (per-project) that contains a `SKILL.md`.

---

## Notes

- `/plugin` commands need a recent Claude Code build with the plugin/marketplace system; `npx` commands need Node.js. The "Manual" blocks need only `git` (or `curl`/`Invoke-WebRequest`, both usually preinstalled).
- PowerShell 5.1+ and PowerShell 7 (`pwsh`) both work for the Windows snippets. macOS/Linux snippets assume bash or zsh.
- Marketplace/plugin names can shift upstream — if `/plugin install` 404s, run `/plugin marketplace update <name>` first, or re-add the marketplace.
- This is a snapshot of one setup, not an endorsement of every skill listed — vet anything before installing it, especially third-party skill packs (they run with the same trust as the instructions you give Claude).
