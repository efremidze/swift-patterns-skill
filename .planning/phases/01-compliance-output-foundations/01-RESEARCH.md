# Phase 1: Compliance + Output Foundations - Research

**Researched:** 2026-01-25
**Domain:** Agent Skills spec compliance, skill documentation structure, output templates, and citation constraints
**Confidence:** MEDIUM

## Summary

This phase is documentation-centric: the skill must conform to the Agent Skills spec, with a valid `SKILL.md` that includes required YAML frontmatter and clear procedural instructions. The official spec requires a `SKILL.md` at the skill root, YAML frontmatter with `name` and `description`, and recommends shallow, one-level file references for efficient context loading. This phase should ensure the existing `swift-patterns/SKILL.md` fully complies with those requirements and exposes the compliance and workflow outputs the planner will rely on.

The phase also establishes two project-specific invariants: a single authoritative constraints section referenced by all workflows, and a citation allowlist rule that restricts references to sources listed in `/references/`. These are not in the upstream spec, so they must be encoded as explicit constraints and enforced by the response templates. Output templates for refactor and review should be standardized, concise, and include explicit constraints/citation checks.

**Primary recommendation:** Make `swift-patterns/SKILL.md` the authoritative entry point with required YAML frontmatter, a single constraints section, and refactor/review response templates that explicitly enforce the citations allowlist in `swift-patterns/references/`.

## Standard Stack

The established libraries/tools for this domain:

### Core
| Library | Version | Purpose | Why Standard |
|---------|---------|---------|--------------|
| Agent Skills Open Format (SKILL.md spec) | N/A | Defines required skill packaging and frontmatter | Official spec for interoperability and validation | 
| Markdown + YAML frontmatter | N/A | Skill instructions and metadata format | Required by Agent Skills spec | 

### Supporting
| Library | Version | Purpose | When to Use |
|---------|---------|---------|-------------|
| skills-ref | N/A (reference tool) | Validate SKILL.md compliance and generate skill prompt XML | Use during authoring/verification | 

### Alternatives Considered
| Instead of | Could Use | Tradeoff |
|------------|-----------|----------|
| README-only guidance | Custom README schema | Violates Agent Skills spec and breaks automated discovery | 
| Deep reference chains | Many nested references | Increases context load complexity; spec recommends one-level references | 

**Installation:**
```bash
# No runtime install required for documentation-only changes.
# Optional validation tool (skills-ref) is a separate, non-production reference utility.
```

## Architecture Patterns

### Recommended Project Structure
```
swift-patterns/
├── SKILL.md                 # Required entry point and constraints/templates
└── references/              # Allowlisted source documents
    ├── concurrency.md
    ├── state.md
    └── ...
```

### Pattern 1: Single Authoritative Constraints Section
**What:** One constraints section in `SKILL.md` that all workflows point to.
**When to use:** Always; required by CORE-06 to prevent scope drift.
**Example:**
```markdown
## Constraints
- Swift-only scope (see AGENTS.md for exclusions)
- No architecture mandates; no tool-specific steps

### Review Workflow
- Follow the Constraints section above
```

### Pattern 2: Standardized Refactor/Review Output Templates
**What:** Dedicated response templates for refactor and review outputs with fixed headings and required checks.
**When to use:** Any refactor/review response; required by CORE-05.
**Example:**
```markdown
## Refactor Response Template
1) Intent + scope
2) Changes (bullet list)
3) Risk checks (constraints + citation allowlist)
4) Next steps
```

### Pattern 3: Citation Allowlist via /references/
**What:** Explicit rule that citations must only reference links present in `references/` files.
**When to use:** Any answer that cites sources; required by COMP-02.
**Example:**
```markdown
## Citations
Only cite URLs listed in references/*.md. If a source is not in references, do not cite it.
```

### Anti-Patterns to Avoid
- **Duplicated constraints across files:** Causes drift and conflicts with CORE-06; keep one authoritative section.
- **Missing frontmatter or invalid `name`:** Breaks spec compliance and automated discovery.
- **Citing external URLs not listed in `references/`:** Violates COMP-02; must enforce allowlist.

## Don't Hand-Roll

Problems that look simple but have existing solutions:

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Spec validation | Custom YAML/frontmatter validator | `skills-ref validate` | Official reference tool matches spec constraints |
| Skill metadata discovery | Custom parsing script | `skills-ref read-properties` | Standardizes name/description parsing |

**Key insight:** Spec compliance is strict and machine-validated; using the reference tool avoids subtle schema mismatches.

## Common Pitfalls

### Pitfall 1: Invalid SKILL.md frontmatter
**What goes wrong:** Missing required fields or invalid `name` format; skill fails validation.
**Why it happens:** Hand-edited YAML without spec checks.
**How to avoid:** Ensure `name` matches directory and conforms to spec; validate with skills-ref.
**Warning signs:** Spec validation errors; name contains uppercase or consecutive hyphens.

### Pitfall 2: Constraints scattered across workflows
**What goes wrong:** Conflicting guidance and scope drift.
**Why it happens:** Copying constraint lists into multiple sections.
**How to avoid:** Single constraints section referenced by all workflows (CORE-06).
**Warning signs:** Different do/don't lists across sections.

### Pitfall 3: Citations outside allowlist
**What goes wrong:** Outputs cite URLs not present in `/references/`, violating COMP-02.
**Why it happens:** No explicit citation rule or template check.
**How to avoid:** Add a citation rule and checklist in templates; keep allowlist in references.
**Warning signs:** Citations to external URLs not found in `swift-patterns/references/`.

## Code Examples

Verified patterns from official sources:

### SKILL.md Frontmatter (Required)
```yaml
# Source: https://agentskills.io/specification.md
---
name: swift-patterns
description: Expert guidance for Swift, SwiftUI, and iOS engineering.
---
```

### File Reference (One-Level Deep)
```markdown
# Source: https://agentskills.io/specification.md
See [the reference guide](references/concurrency.md) for details.
```

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| README-only guidance | `SKILL.md` with YAML frontmatter | Agent Skills spec | Enables automated discovery/validation |
| Deep reference chains | One-level references from `SKILL.md` | Agent Skills spec | Faster context loading and fewer broken links |

**Deprecated/outdated:**
- README-only skill definitions: Not valid per Agent Skills spec; requires `SKILL.md` with frontmatter.

## Open Questions

1. **Citation allowlist contents**
   - What we know: COMP-02 requires citations to be limited to URLs listed in `/references/`.
   - What's unclear: Which authoritative URLs should be added to `swift-patterns/references/*.md` as the allowlist.
   - Recommendation: Define and add a dedicated “Sources” section in each reference file before enforcing citations.

## Sources

### Primary (HIGH confidence)
- https://agentskills.io/specification.md — SKILL.md format, frontmatter rules, file references
- https://raw.githubusercontent.com/agentskills/agentskills/main/skills-ref/README.md — reference validator usage
- /Users/home/Documents/GitHub/swift-skills/AGENTS.md — project constraints for skill content
- /Users/home/Documents/GitHub/swift-skills/.planning/REQUIREMENTS.md — COMP-01/COMP-02/CORE-05/CORE-06 definitions

### Secondary (MEDIUM confidence)
- /Users/home/Documents/GitHub/swift-skills/.planning/research/ARCHITECTURE.md — internal synthesis of workflow/constraints patterns

### Tertiary (LOW confidence)
- None

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH - directly specified by Agent Skills spec
- Architecture: MEDIUM - derived from requirements and internal synthesis
- Pitfalls: MEDIUM - grounded in spec rules and project constraints

**Research date:** 2026-01-25
**Valid until:** 2026-02-24
