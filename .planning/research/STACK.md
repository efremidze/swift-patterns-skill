# Stack Research

**Domain:** Agent Skill (Swift/SwiftUI guidance)
**Researched:** 2026-01-25
**Confidence:** MEDIUM

## Recommended Stack

### Core Technologies

| Technology | Version | Purpose | Why Recommended |
|------------|---------|---------|-----------------|
| Agent Skills Open Format (SKILL.md spec) | N/A (spec is not versioned) | Interoperable skill packaging | Required by the open spec for cross-agent compatibility; avoids vendor lock-in. Confidence: HIGH (spec).
| Markdown body content | N/A (spec-defined) | Primary instruction content for agents | The spec mandates Markdown instructions; aligns with broad agent support. Confidence: HIGH (spec).
| YAML frontmatter | N/A (spec-defined) | Machine-readable metadata (name, description, etc.) | Required by the spec for discovery and activation; enables consistent indexing. Confidence: HIGH (spec).

### Supporting Libraries

| Library | Version | Purpose | When to Use |
|---------|---------|---------|-------------|
| skills-ref | 0.1.0 | Validate SKILL.md and generate prompt XML | Use during authoring to validate spec compliance; note this is a reference/demo tool, not production-grade. Confidence: HIGH (skills-ref pyproject/README).

### Development Tools

| Tool | Purpose | Notes |
|------|---------|-------|
| Python | >= 3.11 | Run skills-ref validator | Required by skills-ref; keep local-only for validation workflows. Confidence: HIGH (skills-ref pyproject).
| Git | N/A (current stable) | Versioned distribution of the skill repo | De facto distribution channel for Agent Skills; use tags/releases for versioning. Confidence: MEDIUM (ecosystem practice).

## Installation

```bash
# Core
# No runtime packages required. Agent Skills are Markdown + YAML in a git repo.

# Supporting (optional validation)
# Follow skills-ref README to install from source if needed.
```

## Alternatives Considered

| Recommended | Alternative | When to Use Alternative |
|-------------|-------------|-------------------------|
| Agent Skills Open Format | Vendor-specific skill formats | Use only if distributing exclusively to a single product and you can accept lock-in. Confidence: MEDIUM (ecosystem practice).
| skills-ref (reference validator) | Custom validation scripts | Use custom scripts if you need stricter rules than the reference spec or CI automation at scale. Confidence: MEDIUM.

## What NOT to Use

| Avoid | Why | Use Instead |
|-------|-----|-------------|
| README-only guidance without SKILL.md frontmatter | Violates the spec and breaks automated discovery. Confidence: HIGH (spec). | SKILL.md with required YAML frontmatter.
| Deep reference chains across many files | Spec recommends shallow, one-level references for efficient context loading. Confidence: HIGH (spec). | Keep references one level deep and focused.

## Stack Patterns by Variant

**If distributing across multiple agent products:**
- Use the open Agent Skills format with minimal product-specific fields.
- Because interoperability is the primary value of the spec.

**If targeting a single agent product only:**
- Use the `compatibility` field to document that environment.
- Because some agents may need explicit tool or runtime constraints.

## Version Compatibility

| Package A | Compatible With | Notes |
|-----------|-----------------|-------|
| skills-ref@0.1.0 | Python >= 3.11 | Minimum Python version required by the reference tool. |

## Sources

- https://agentskills.io/specification.md — SKILL.md structure, YAML frontmatter, Markdown body, reference guidance (HIGH)
- https://raw.githubusercontent.com/agentskills/agentskills/main/skills-ref/pyproject.toml — skills-ref version + Python requirement (HIGH)
- https://raw.githubusercontent.com/agentskills/agentskills/main/skills-ref/README.md — skills-ref usage scope (demo-only) (HIGH)

---
*Stack research for: Agent Skill (Swift/SwiftUI guidance)*
*Researched: 2026-01-25*
