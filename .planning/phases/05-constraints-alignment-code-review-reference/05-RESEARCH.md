# Phase 5: Constraints Alignment for Code Review Reference - Research

**Researched:** 2026-01-26
**Domain:** Swift skill documentation alignment (constraints enforcement for code review reference)
**Confidence:** HIGH

## Summary

This phase is a documentation alignment task inside the Swift skill. The code review/refactor reference currently violates the skill Constraints and is missing a constraints link. Planning must focus on making `swift-agent-skill/references/code-review-refactoring.md` comply with the single Constraints section in `swift-agent-skill/SKILL.md`, while preserving the Quick Decision Guide linkage.

The standard approach in this repo is a single authoritative Constraints section in `swift-agent-skill/SKILL.md`, referenced by all workflows and references. The code review/refactor reference should mirror other references by adding a required Constraints link, removing disallowed content (tool-specific steps, formatting rules, UIKit examples), and keeping scope within Swift/SwiftUI guidance. This is a gap-closure fix identified in the milestone audit.

**Primary recommendation:** Update `swift-agent-skill/references/code-review-refactoring.md` to include a mandatory Constraints link and remove all disallowed content to preserve the Quick Decision Guide flow.

## Standard Stack

The established documentation sources for this phase:

### Core
| Reference | Version | Purpose | Why Standard |
| --- | --- | --- | --- |
| `swift-agent-skill/SKILL.md` | repo | Single Constraints section | Authoritative constraints source used by all workflows |
| `swift-agent-skill/references/code-review-refactoring.md` | repo | Code review/refactor reference | Target file linked from Quick Decision Guide |

### Supporting
| Reference | Version | Purpose | When to Use |
| --- | --- | --- | --- |
| `swift-agent-skill/references/workflows-review.md` | repo | Constraints-linked workflow structure | Use as pattern for required Constraints link |
| `swift-agent-skill/references/workflows-refactor.md` | repo | Constraints-linked workflow structure | Use as pattern for required Constraints link |
| `swift-agent-skill/references/decisions.md` | repo | Routing and shared constraints note | Ensure alignment with shared constraints language |

### Alternatives Considered
| Instead of | Could Use | Tradeoff |
| --- | --- | --- |
| Embedding constraints inside each reference | Separate constraints per file | Violates single-source constraints and risks drift |

## Architecture Patterns

### Recommended Documentation Structure
Keep Constraints in `swift-agent-skill/SKILL.md` and link to them from references.

### Pattern 1: Required Constraints Link in References
**What:** Each reference starts with a required Constraints link, mirroring workflow references.
**When to use:** Any reference that can be reached via the Quick Decision Guide or workflow routing.
**Example:**
```markdown
**Required references:**
- **Constraints (mandatory):** See [SKILL.md Constraints](../SKILL.md#constraints).
```

### Anti-Patterns to Avoid
- **Duplicating constraints per reference:** Causes drift and inconsistent enforcement.
- **Leaving disallowed guidance in references:** Breaks constraints compliance for linked flows.

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
| --- | --- | --- | --- |
| Constraints enforcement in references | Custom constraints blocks in each reference | Link to `swift-agent-skill/SKILL.md#constraints` | Single authoritative source prevents drift |
| Tooling guidance | Tool-specific steps (Xcode, Instruments) | Tool-agnostic guidance | Constraints forbid tool-specific steps |
| Style enforcement | Formatting/linting rules | Quality guidance without style mandates | Constraints forbid formatting rules |
| UIKit examples | UIKit-based sample code | Swift/SwiftUI examples only | Constraints exclude UIKit unless bridging |

**Key insight:** This phase is about removing out-of-scope content and linking to the single Constraints section, not inventing new rules.

## Common Pitfalls

### Pitfall 1: Missing Constraints Link
**What goes wrong:** The reference remains reachable from Quick Decision Guide without constraints enforcement.
**Why it happens:** The file predates the single-constraints pattern.
**How to avoid:** Add a required Constraints link near the top, matching other references.
**Warning signs:** The reference lacks a `SKILL.md#constraints` link.

### Pitfall 2: Tool-Specific Guidance Left Behind
**What goes wrong:** The reference still instructs tool usage (e.g., profiling tools).
**Why it happens:** Legacy guidance included tools before constraints existed.
**How to avoid:** Remove tool-specific steps and keep guidance tool-agnostic.
**Warning signs:** Mentions of Instruments, IDE workflows, or CLI steps.

### Pitfall 3: Formatting Rules Embedded in Review Checklist
**What goes wrong:** The reference enforces style or formatting consistency.
**Why it happens:** Review checklists often include style checks by default.
**How to avoid:** Remove formatting/linting rules and focus on correctness and maintainability.
**Warning signs:** Checklist items like "formatting consistent" or "naming conventions."

### Pitfall 4: UIKit Examples Persist
**What goes wrong:** The reference uses UIKit classes like `ViewController` in examples.
**Why it happens:** Generic refactoring examples were reused.
**How to avoid:** Replace with Swift/SwiftUI-focused examples or abstract pseudocode.
**Warning signs:** `UIViewController`, `ViewController`, or UIKit patterns.

## Code Examples

Verified patterns from internal references:

### Required Constraints Link Pattern
```markdown
**Required references:**
- **Constraints (mandatory):** See [SKILL.md Constraints](../SKILL.md#constraints).
```

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
| --- | --- | --- | --- |
| Reference files embed their own guidance without constraints links | References link to single Constraints section in `SKILL.md` | Phase 1 compliance foundation | Prevents scope drift and enforces shared rules |

**Deprecated/outdated:**
- Tool-specific review steps in references: replaced by tool-agnostic guidance per Constraints.

## Open Questions

1. **Should remaining non-SwiftUI examples be replaced or removed?**
   - What we know: Current examples include non-SwiftUI and UIKit usage in `references/code-review-refactoring.md`.
   - What's unclear: Whether to replace with SwiftUI-specific examples or remove large example blocks entirely.
   - Recommendation: Prefer SwiftUI-focused examples or concise pseudocode that avoids UIKit and tool references.

## Sources

### Primary (HIGH confidence)
- `swift-agent-skill/SKILL.md` - Constraints section and Quick Decision Guide linkage
- `swift-agent-skill/references/code-review-refactoring.md` - Current reference content
- `swift-agent-skill/references/workflows-review.md` - Required Constraints link pattern
- `swift-agent-skill/references/workflows-refactor.md` - Required Constraints link pattern
- `.planning/v1.0-MILESTONE-AUDIT.md` - Gap description and required alignment

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH - all sources are internal repo docs
- Architecture: HIGH - patterns are already established in workflow references
- Pitfalls: HIGH - directly observed in current reference content

**Research date:** 2026-01-26
**Valid until:** 2026-02-25
