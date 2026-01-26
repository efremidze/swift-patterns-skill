# Coding Conventions

**Analysis Date:** 2026-01-25

## Naming Patterns

**Files:**
- Markdown references: lowercase kebab-case (e.g. `concurrency.md` in `swift-patterns/references/`)
- Root docs: uppercase (e.g. `README.md`, `CONTRIBUTING.md`)
- Hook scripts: kebab-case with `.js` (e.g. `.opencode/hooks/gsd-statusline.js`)

**Functions:**
- JavaScript functions use camelCase (e.g. `process.stdin.on`, `fs.readFileSync` usage in `.opencode/hooks/gsd-statusline.js`)

**Variables:**
- JavaScript variables use lowerCamelCase and `const` where possible (see `.opencode/hooks/gsd-check-update.js`)

**Types:**
- Not applicable (no TypeScript/types)

## Code Style

**Formatting:**
- No formatter config detected

**Linting:**
- No lint config detected

## Import Organization

**Order:**
1. Built-in Node.js modules via `require` (e.g. `fs`, `path`, `os` in `.opencode/hooks/gsd-statusline.js`)

**Path Aliases:**
- Not detected

## Error Handling

**Patterns:**
- Silent failure via empty `catch` blocks (e.g. `.opencode/hooks/gsd-check-update.js`)
- Try/catch around JSON parsing in `.opencode/hooks/gsd-statusline.js`

## Logging

**Framework:** None

**Patterns:**
- Scripts avoid logging; output is reserved for statusline rendering

## Comments

**When to Comment:**
- Header comments describe script purpose and behavior (e.g. `.opencode/hooks/gsd-statusline.js`)

**JSDoc/TSDoc:**
- Not detected

## Function Design

**Size:**
- Small utility scripts with a single entry point (`process.stdin.on('end', ...)`)

**Parameters:**
- Standard Node.js callbacks and configuration objects

**Return Values:**
- Output via stdout or filesystem writes

## Module Design

**Exports:**
- Not detected (scripts are executables, not modules)

**Barrel Files:**
- Not detected

---

*Convention analysis: 2026-01-25*
