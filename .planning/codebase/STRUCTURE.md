# Codebase Structure

**Analysis Date:** 2026-01-25

## Directory Layout

```
[project-root]/
├── swift-agent-skill/      # Skill definition and references
│   ├── SKILL.md             # Main workflow/decision tree
│   └── references/          # Topic-specific guidance
├── .opencode/               # OpenCode automation hooks
│   └── hooks/               # Node.js hook scripts
├── .claude/                 # Claude-specific workflows/hooks
│   └── hooks/               # Node.js hook scripts
├── .planning/               # Planning artifacts (created by GSD)
├── README.md                # Repository documentation
├── package.json             # npm metadata
├── package-lock.json        # npm lockfile
└── AGENTS.md                # Agent behavior guidelines
```

## Directory Purposes

**swift-agent-skill/**
- Purpose: Core skill content
- Contains: `SKILL.md`, `references/*.md`
- Key files: `swift-agent-skill/SKILL.md`, `swift-agent-skill/references/concurrency.md`

**.opencode/**
- Purpose: OpenCode automation assets
- Contains: hook scripts in `.opencode/hooks/`
- Key files: `.opencode/hooks/gsd-check-update.js`, `.opencode/hooks/gsd-statusline.js`

**.claude/**
- Purpose: Claude workflows, hooks, and templates
- Contains: `.claude/hooks/` and GSD workflow definitions
- Key files: `.claude/agents/gsd-codebase-mapper.md`, `.claude/get-shit-done/workflows/map-codebase.md`

**.planning/**
- Purpose: GSD planning artifacts (generated)
- Contains: `.planning/codebase/*.md` and future planning docs

## Key File Locations

**Entry Points:**
- `swift-agent-skill/SKILL.md`: Skill overview and workflow
- `README.md`: Repository usage and install instructions

**Configuration:**
- `package.json`: npm metadata
- `package-lock.json`: npm lockfile
- `AGENTS.md`: Agent constraints

**Core Logic:**
- `.opencode/hooks/gsd-check-update.js`: Update check hook
- `.opencode/hooks/gsd-statusline.js`: Statusline hook

**Testing:**
- Not detected

## Naming Conventions

**Files:**
- Markdown references use lowercase with hyphens, e.g. `concurrency.md`
- Root docs use uppercase names, e.g. `README.md`, `CONTRIBUTING.md`

**Directories:**
- Feature grouping by purpose, e.g. `swift-agent-skill/references/`

## Where to Add New Code

**New Skill Content:**
- Primary docs: `swift-agent-skill/`
- References: `swift-agent-skill/references/`

**New Hooks:**
- OpenCode hooks: `.opencode/hooks/`
- Claude hooks: `.claude/hooks/`

**Utilities:**
- Not detected (no shared code utilities directory)

## Special Directories

**.planning/**
- Purpose: Generated planning artifacts
- Generated: Yes
- Committed: Depends on workflow config

---

*Structure analysis: 2026-01-25*
