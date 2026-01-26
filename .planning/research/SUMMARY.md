# Project Research Summary

**Project:** Swift Expert Skill Refactor
**Domain:** Agent Skill for Swift/SwiftUI refactor and review guidance
**Researched:** 2026-01-25
**Confidence:** MEDIUM

## Executive Summary

This project is an Agent Skill package that gives AI coding agents concise, modern Swift/SwiftUI refactor and review guidance. Experts build this type of skill by using the Agent Skills Open Format with strict scope constraints, decision logic to route requests (review vs refactor), and modular topic units to keep rules consistent over time.

The recommended approach is to ship a minimal, decision-gated skill: start with explicit scope boundaries, a review/refactor decision tree, and core SwiftUI guidance (state ownership, navigation, lists, and baseline performance). Use modular topic files referenced by workflows, and maintain a modern API replacement list to keep guidance current.

Key risks are scope creep into non-SwiftUI topics, architecture mandates disguised as best practices, and outdated API advice. Mitigate with a single authoritative constraints section referenced everywhere, a verification checklist for API currency, and careful language that frames optimizations as conditional suggestions.

## Key Findings

### Recommended Stack

The stack is intentionally lightweight: the Agent Skills Open Format with Markdown content and YAML frontmatter is required for interoperability. Use the reference validator (skills-ref) during authoring, with Python 3.11+, but keep runtime dependencies minimal.

**Core technologies:**
- Agent Skills Open Format (SKILL.md spec): interoperable skill packaging — required for cross-agent compatibility.
- Markdown body content: primary instruction content — mandated by the spec and widely supported.
- YAML frontmatter: machine-readable metadata — required for discovery and activation.

### Expected Features

The MVP is guidance-heavy and workflow-driven: state management, navigation, lists, refactor safety, and review checklists are expected. Differentiators are decision trees and a modern API replacement catalog. Defer high-effort playbooks and case studies until the core guidance is validated.

**Must have (table stakes):**
- SwiftUI state management guidance — users expect clear ownership rules.
- Refactor checklist (behavior-preserving) — primary workflow for safe changes.
- Review checklist for SwiftUI — consistent, actionable review output.
- Navigation patterns guidance — common refactor pain point.
- Performance baseline guidance — avoids obvious SwiftUI regressions.

**Should have (competitive):**
- Decision trees for state wrapper selection — faster correct choices.
- Modern API replacement catalog — keeps refactors current.
- Testing/DI guidance (lightweight) — safer refactors without mandating frameworks.

**Defer (v2+):**
- Refactor playbooks by goal — high effort, needs more examples.
- Before/after case studies — best once patterns stabilize.
- Performance tuning recipes — only after baseline rules settle.

### Architecture Approach

Architecture should be modular and decision-gated: constraints and glossary first, then decision logic, then workflows that reference topic units, all culminating in consistent output templates. Keep rules atomic in topic modules to avoid drift.

**Major components:**
1. Constraints and glossary — enforce scope and shared language.
2. Decision logic — route review vs refactor and gate risky requests.
3. Workflows — refactor and review playbooks that link to topics.
4. Topic units — SwiftUI state, navigation, lists, performance, testing/DI rules.
5. Output templates — consistent, high-signal responses.

### Critical Pitfalls

1. **Mixing SwiftUI with non-SwiftUI guidance** — prevent by enforcing strict scope boundaries in every section.
2. **Architecture mandates disguised as best practices** — avoid mandates; use neutral, optional language.
3. **Outdated or deprecated API guidance** — maintain a modern replacement list and verify against current docs.
4. **Optional optimizations treated as requirements** — label performance guidance as conditional.
5. **Missing decision logic for review vs refactor** — add explicit intent routing to avoid mis-scoped advice.

## Implications for Roadmap

Based on research, suggested phase structure:

### Phase 1: Scope + Foundations
**Rationale:** Prevent scope creep and set shared language before content expands.
**Delivers:** Constraints, glossary, output templates, and high-level intent framing.
**Addresses:** Review checklist baseline, refactor checklist framing (FEATURES).
**Avoids:** Scope creep into non-SwiftUI topics; contradictory rules.

### Phase 2: Decision Logic + Core Workflows
**Rationale:** Routing must exist before adding detailed guidance; workflows drive usability.
**Delivers:** Review/refactor decision tree, core workflows, and links into topic units.
**Addresses:** SwiftUI state management, navigation patterns, list best practices, review checklist, refactor checklist.
**Avoids:** Missing decision logic; architecture mandates.

### Phase 3: Modernization + Baseline Performance
**Rationale:** Once workflows exist, stabilize correctness with modern APIs and baseline performance rules.
**Delivers:** Modern API replacement catalog, performance baseline guidance, lightweight testing/DI guidance.
**Addresses:** Performance baseline guidance, testing/DI guidance, modern API catalog.
**Avoids:** Deprecated API guidance; optional optimizations treated as mandates.

### Phase 4: Enhancements + Consistency Audit
**Rationale:** Advanced content should only follow validated core guidance.
**Delivers:** Decision trees for state wrappers, refactor playbooks, case studies, performance tuning recipes, consistency audit.
**Addresses:** Differentiators and v2 features.
**Avoids:** Contradictory instructions across files; unverifiable claims.

### Phase Ordering Rationale

- Constraints and decision logic are dependencies for all content and prevent scope drift.
- Workflows should be built before adding more topic depth to avoid mismatched guidance.
- Modern API and performance rules must follow workflow stabilization to avoid churn.

### Research Flags

Phases likely needing deeper research during planning:
- **Phase 3:** API modernization requires up-to-date SwiftUI deprecation checks.
- **Phase 4:** Playbooks and case studies need validated examples and evidence.

Phases with standard patterns (skip research-phase):
- **Phase 1:** Constraints/glossary/templates are well-established in skill packaging.
- **Phase 2:** Decision logic + workflow structure is standard for agent skills.

## Confidence Assessment

| Area | Confidence | Notes |
|------|------------|-------|
| Stack | MEDIUM | Spec sources are strong; ecosystem usage patterns are inferred. |
| Features | MEDIUM | Based on official docs and domain expectations. |
| Architecture | LOW | Internal synthesis only; needs validation. |
| Pitfalls | MEDIUM | Grounded in repo constraints and common failures. |

**Overall confidence:** MEDIUM

### Gaps to Address

- **SwiftUI API currency:** verify deprecations and replacements against current Apple docs before finalizing guidance.
- **Architecture fit to current repo:** validate that the proposed module split aligns with existing `swift-agent-skill/` content.
- **Concurrency scope:** reconcile topic coverage with the “no deep concurrency patterns” constraint.

## Sources

### Primary (HIGH confidence)
- https://agentskills.io/specification.md — Agent Skills spec structure and requirements
- https://developer.apple.com/documentation/swiftui — SwiftUI reference
- https://developer.apple.com/documentation/observation — Observation framework (`@Observable`) reference

### Secondary (MEDIUM confidence)
- https://developer.apple.com/documentation/swiftui/navigationstack — NavigationStack guidance
- https://developer.apple.com/documentation/xctest — XCTest basics for lightweight testing seams
- https://raw.githubusercontent.com/agentskills/agentskills/main/skills-ref/pyproject.toml — skills-ref version and Python requirement
- https://raw.githubusercontent.com/agentskills/agentskills/main/skills-ref/README.md — skills-ref scope

### Tertiary (LOW confidence)
- .planning/research/ARCHITECTURE.md — internal architecture synthesis
- .planning/research/PITFALLS.md — pitfalls and recovery mapping
- AGENTS.md — project constraints

---
*Research completed: 2026-01-25*
*Ready for roadmap: yes*
