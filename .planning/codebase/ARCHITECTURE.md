# Architecture

**Analysis Date:** 2026-01-25

## Pattern Overview

**Overall:** Documentation repository with lightweight Node.js hook scripts.

**Key Characteristics:**
- Skill content lives in Markdown under `swift-expert-skill/`
- Tooling hooks live in `.opencode/hooks/` and `.claude/hooks/`
- No application runtime or build pipeline present

## Layers

**Skill Content:**
- Purpose: Primary deliverable (Swift skill guidance)
- Location: `swift-expert-skill/`
- Contains: `SKILL.md`, reference documents in `swift-expert-skill/references/*.md`
- Depends on: None
- Used by: Documentation consumers and agent skill tooling

**Project Docs:**
- Purpose: Repository-level documentation
- Location: `README.md`, `CONTRIBUTING.md`, `LICENSE`
- Contains: Usage, contribution, license
- Depends on: None
- Used by: End users and contributors

**Automation Hooks:**
- Purpose: Statusline and update checks for GSD workflows
- Location: `.opencode/hooks/`, `.claude/hooks/`
- Contains: Node.js scripts (see `.opencode/hooks/gsd-check-update.js`, `.opencode/hooks/gsd-statusline.js`)
- Depends on: Node.js runtime, local filesystem
- Used by: Claude/Opencode hook systems

## Data Flow

**GSD Update Check:**
1. `.opencode/hooks/gsd-check-update.js` resolves local version file from `.claude/get-shit-done/VERSION`.
2. Script shells out to `npm view get-shit-done-cc version`.
3. Result is cached to `~/.claude/cache/gsd-update-check.json`.

**Statusline Render:**
1. `.opencode/hooks/gsd-statusline.js` reads JSON from stdin.
2. Script reads todos from `~/.claude/todos` for current session.
3. Script reads update cache from `~/.claude/cache/gsd-update-check.json`.
4. Statusline is written to stdout.

**State Management:**
- State is file-based (JSON cache and session todos in the user home directory).

## Key Abstractions

**Skill Document:**
- Purpose: Defines the workflow and references for the Swift skill
- Examples: `swift-expert-skill/SKILL.md`
- Pattern: Markdown-based declarative documentation

**Reference Document:**
- Purpose: Deep-dive guidance on a specific topic
- Examples: `swift-expert-skill/references/swift-concurrency.md`, `swift-expert-skill/references/performance.md`
- Pattern: Markdown guides with sectioned content

## Entry Points

**Skill Overview:**
- Location: `swift-expert-skill/SKILL.md`
- Triggers: Loaded by agent tooling that supports Agent Skills format
- Responsibilities: Provide decision tree and references

**Repository Readme:**
- Location: `README.md`
- Triggers: Viewed by users on GitHub or after clone
- Responsibilities: Installation and usage instructions

**Hook Scripts:**
- Location: `.opencode/hooks/gsd-check-update.js`, `.opencode/hooks/gsd-statusline.js`
- Triggers: Hook system invokes scripts
- Responsibilities: Update checks and statusline rendering

## Error Handling

**Strategy:** Best-effort, silent failure

**Patterns:**
- `try/catch` with empty handlers in `.opencode/hooks/gsd-check-update.js`
- Silent failure on JSON parse in `.opencode/hooks/gsd-statusline.js`

## Cross-Cutting Concerns

**Logging:** None (scripts are intentionally quiet)
**Validation:** Minimal (JSON parse guarded)
**Authentication:** None

---

*Architecture analysis: 2026-01-25*
