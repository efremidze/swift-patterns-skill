# Roadmap: Swift Expert Skill Refactor

## Overview

This roadmap delivers a decision-gated Swift/SwiftUI skill that agents can use immediately for review and refactor work. It starts with compliant packaging and response templates, then adds workflow decisioning and safety checks before expanding into SwiftUI guidance, modernization, and quality playbooks.

## Phases

**Phase Numbering:**
- Integer phases (1, 2, 3): Planned milestone work
- Decimal phases (2.1, 2.2): Urgent insertions (marked with INSERTED)

Decimal phases appear between their surrounding integers in numeric order.

- [x] **Phase 1: Compliance + Output Foundations** - Skill packaging, constraints, and standardized response templates are usable.
- [x] **Phase 2: Decisioned Workflows + Safety** - Review/refactor routing and risk-aware checklists are in place.
- [x] **Phase 3: SwiftUI Guidance Core** - Core SwiftUI and lightweight concurrency guidance is available.
- [x] **Phase 4: Quality + Playbooks** - Quality guidance and refactor playbooks complete v1.
- [x] **Phase 5: Constraints Alignment for Code Review Reference** - Align code review reference with constraints and remove disallowed content.

## Phase Details

### Phase 1: Compliance + Output Foundations
**Goal**: The skill loads correctly and provides constraints plus standardized refactor/review response templates.
**Depends on**: Nothing (first phase)
**Requirements**: COMP-01, COMP-02, CORE-05, CORE-06
**Success Criteria** (what must be TRUE):
  1. User can load the skill and see required metadata and procedural instructions in `SKILL.md`.
  2. User can see refactor and review response templates that standardize agent output format.
  3. User can verify citations are restricted to sources listed in `/references/`.
  4. User can find a single constraints section referenced by all workflows.
**Plans**: 2 plans

Plans:
- [x] 01-01-PLAN.md — Define skill metadata, constraints, and citation rules
- [x] 01-02-PLAN.md — Establish standardized response templates

### Phase 2: Decisioned Workflows + Safety
**Goal**: Users can route requests into refactor vs review workflows with consistent, risk-aware checklists and a shared constraints section.
**Depends on**: Phase 1
**Requirements**: CORE-01, CORE-02, CORE-03, CORE-04, CORE-07
**Success Criteria** (what must be TRUE):
  1. User can determine whether a request should follow review or refactor workflow using intent cues.
  2. User can apply a behavior-preserving refactor checklist for SwiftUI changes.
  3. User can apply a SwiftUI review checklist and receive consistent findings.
  4. User can identify when a refactor should be split or when tests should be added first.
  5. User can apply an invariants list that preserves identity and data flow during refactors.
**Plans**: 2 plans

Plans:
- [x] 02-01-PLAN.md — Build workflow routing and shared constraints section
- [x] 02-02-PLAN.md — Add refactor/review checklists with risk cues and invariants

### Phase 3: SwiftUI Guidance Core
**Goal**: Users can apply core SwiftUI guidance for state, navigation, lists, composition, layout, scrolling, and lightweight concurrency.
**Depends on**: Phase 2
**Requirements**: SWUI-01, SWUI-02, SWUI-03, SWUI-04, SWUI-05, SWUI-06, CONC-01, CONC-02, MOD-01, MOD-02
**Success Criteria** (what must be TRUE):
  1. User can select the appropriate state wrapper using ownership guidance and decision trees.
  2. User can implement navigation and presentation patterns with `NavigationStack` and destinations.
  3. User can build lists/collections with stable identity and appropriate lazy containers.
  4. User can structure view composition and data flow with layout, alignment, spacing, and adaptive patterns.
  5. User can apply scrolling patterns with safe pagination triggers and async work using `.task` and `@MainActor` guidance.
  6. User can replace deprecated SwiftUI APIs using a modern replacement catalog.
**Plans**: 2 plans

Plans:
- [x] 03-01-PLAN.md — Draft SwiftUI state, navigation, lists, composition, layout guidance, and API replacements
- [x] 03-02-PLAN.md — Add scroll and lightweight concurrency guidance with decision trees

### Phase 4: Quality + Playbooks
**Goal**: Users can apply quality, performance, and refactor playbooks safely.
**Depends on**: Phase 3
**Requirements**: PERF-01, PERF-02, TEST-01, PLAY-01
**Success Criteria** (what must be TRUE):
  1. User can avoid common performance pitfalls with baseline SwiftUI guidance.
  2. User can apply best-practice patterns for identity stability and expensive work avoidance.
  3. User can introduce lightweight testing/DI seams to reduce refactor risk.
  4. User can follow goal-based refactor playbooks for common migrations.
**Plans**: 2 plans

Plans:
- [x] 04-01-PLAN.md — Publish performance baseline and testing/DI seams guidance
- [x] 04-02-PLAN.md — Add goal-based refactor playbooks and workflow wiring

### Phase 5: Constraints Alignment for Code Review Reference
**Goal**: Code review/refactor reference content complies with constraints and keeps Quick Decision Guide flows safe.
**Depends on**: Phase 4
**Requirements**: (gap closure)
**Success Criteria** (what must be TRUE):
  1. Quick Decision Guide links to a code review/refactor reference that enforces constraints.
  2. Tool-specific steps, formatting rules, and UIKit examples are removed from that reference.
  3. The reference links back to the Constraints section for scope enforcement.
**Plans**: 1 plan

Plans:
- [x] 05-01-PLAN.md — Align code review reference with constraints

## Progress

**Execution Order:**
Phases execute in numeric order: 2 → 2.1 → 2.2 → 3 → 3.1 → 4 → 5

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Compliance + Output Foundations | 2/2 | Complete | 2026-01-26 |
| 2. Decisioned Workflows + Safety | 2/2 | Complete | 2026-01-26 |
| 3. SwiftUI Guidance Core | 2/2 | Complete | 2026-01-26 |
| 4. Modernization + Quality Playbooks | 2/2 | Complete | 2026-01-26 |
| 5. Constraints Alignment for Code Review Reference | 1/1 | Complete | 2026-01-26 |
