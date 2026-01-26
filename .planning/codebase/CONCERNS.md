# Codebase Concerns

**Analysis Date:** 2026-01-25

## Tech Debt

**Duplicate Hook Scripts:**
- Issue: Same hook logic exists in two locations and can drift
- Files: `.opencode/hooks/gsd-check-update.js`, `.claude/hooks/gsd-check-update.js`, `.opencode/hooks/gsd-statusline.js`, `.claude/hooks/gsd-statusline.js`
- Impact: Behavior differences across environments if one copy changes
- Fix approach: Consolidate or document the source of truth

## Known Bugs

**Not detected**

## Security Considerations

**External Command Execution:**
- Risk: `npm view` is executed via shell
- Files: `.opencode/hooks/gsd-check-update.js`
- Current mitigation: Command is fixed string, no user input
- Recommendations: Keep command string static and avoid user-supplied input

## Performance Bottlenecks

**Not detected**

## Fragile Areas

**Local Cache Dependence:**
- Files: `.opencode/hooks/gsd-statusline.js`
- Why fragile: Assumes cache and todo directories exist in `~/.claude/`
- Safe modification: Guard file reads and keep try/catch behavior
- Test coverage: No automated tests detected

## Scaling Limits

**Not detected**

## Dependencies at Risk

**Not detected**

## Missing Critical Features

**Not detected**

## Test Coverage Gaps

**No automated tests:**
- What's not tested: Hook script behavior and parsing
- Files: `.opencode/hooks/gsd-check-update.js`, `.opencode/hooks/gsd-statusline.js`
- Risk: Regressions in statusline output or update checks
- Priority: Medium

---

*Concerns audit: 2026-01-25*
