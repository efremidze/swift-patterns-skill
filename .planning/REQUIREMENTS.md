# Requirements: Swift Expert Skill Refactor

**Defined:** 2026-01-25
**Core Value:** AI agents can quickly apply consistent, modern Swift/SwiftUI guidance to refactor and review code without ambiguity.

## v1 Requirements

Requirements for initial release. Each maps to roadmap phases.

### Core Workflows

- [ ] **CORE-01**: Agent can route requests into review vs refactor workflow based on user intent cues
- [ ] **CORE-02**: Refactor checklist defines behavior-preserving steps for SwiftUI changes
- [ ] **CORE-03**: Review checklist produces consistent SwiftUI review findings
- [ ] **CORE-04**: Risk assessment cues indicate when to split refactors or add tests first
- [ ] **CORE-05**: Output templates standardize refactor/review responses for agent usability
- [ ] **CORE-06**: Single constraints section is referenced by all workflows to prevent scope drift
- [ ] **CORE-07**: Identity and data flow invariants list what refactors must preserve (stable IDs, state ownership, navigation source of truth, cancellable async work)

### SwiftUI Guidance

- [x] **SWUI-01**: State ownership guidance covers `@State`, `@Binding`, `@Observable`, and `@Environment`
- [x] **SWUI-02**: Navigation guidance covers `NavigationStack`, destinations, and presentation patterns
- [x] **SWUI-03**: List/collection guidance covers stable identity, `ForEach` pitfalls, and lazy containers
- [x] **SWUI-04**: View composition guidance covers extraction and data flow between parent/child views
- [x] **SWUI-05**: Layout guidance covers stack usage, alignment, spacing, and adaptive layout patterns
- [x] **SWUI-06**: Scroll guidance covers `ScrollView` patterns and safe pagination triggers

### Swift Concurrency (Lightweight)

- [x] **CONC-01**: Async work guidance for SwiftUI uses `.task` and cancellation-aware patterns
- [x] **CONC-02**: UI update guidance clarifies `@MainActor` usage without deep concurrency dives

### Quality & Performance

- [x] **PERF-01**: Performance baseline guidance covers common SwiftUI pitfalls and safe optimizations
- [x] **PERF-02**: Performance best-practice patterns cover view identity stability and expensive work avoidance
- [x] **TEST-01**: Testing/DI guidance shows lightweight seams for refactor safety without tool mandates

### Modernization & Decisioning

- [x] **MOD-01**: Modern API replacement catalog maps deprecated SwiftUI APIs to current equivalents
- [x] **MOD-02**: Decision trees guide selection of state wrappers based on ownership and sharing

### Advanced Playbooks

- [x] **PLAY-01**: Goal-based refactor playbooks guide common migrations (view extraction, navigation, state hoisting)

### Compliance & Packaging

- [ ] **COMP-01**: Skill conforms to Agent Skills spec (metadata + procedural instructions in `SKILL.md`)
- [ ] **COMP-02**: AI-safe citations rule restricts references to links listed in `/references/`

## v2 Requirements

Deferred to future release. Tracked but not in current roadmap.

### Accessibility

- **A11Y-01**: Accessibility basics cover labels, dynamic type, and hit targets

### Examples & Tuning

- **CASE-01**: Before/after case studies show refactor decisions and tradeoffs
- **PERF-03**: Performance tuning recipes provide advanced optimization guidance

## Out of Scope

Explicitly excluded. Documented to prevent scope creep.

| Feature | Reason |
|---------|--------|
| Architecture mandates (MVVM/MVC/VIPER) | Conflicts with non-prescriptive guidance goal |
| Formatting or linting rules | Style policing, not refactor guidance |
| Tool-specific steps (Xcode/Instruments/CLI workflows) | Not usable in all agent environments |
| Generic Swift language tutorials | Dilutes SwiftUI refactor focus |
| UIKit or server-side Swift guidance | Outside SwiftUI-focused scope |

## Traceability

Which phases cover which requirements. Updated during roadmap creation.

| Requirement | Phase | Status |
|-------------|-------|--------|
| CORE-01 | Phase 2 | Complete |
| CORE-02 | Phase 2 | Complete |
| CORE-03 | Phase 2 | Complete |
| CORE-04 | Phase 2 | Complete |
| CORE-05 | Phase 1 | Complete |
| CORE-06 | Phase 1 | Complete |
| CORE-07 | Phase 2 | Complete |
| SWUI-01 | Phase 3 | Complete |
| SWUI-02 | Phase 3 | Complete |
| SWUI-03 | Phase 3 | Complete |
| SWUI-04 | Phase 3 | Complete |
| SWUI-05 | Phase 3 | Complete |
| SWUI-06 | Phase 3 | Complete |
| CONC-01 | Phase 3 | Complete |
| CONC-02 | Phase 3 | Complete |
| PERF-01 | Phase 4 | Complete |
| PERF-02 | Phase 4 | Complete |
| TEST-01 | Phase 4 | Complete |
| MOD-01 | Phase 3 | Complete |
| MOD-02 | Phase 3 | Complete |
| PLAY-01 | Phase 4 | Complete |
| COMP-01 | Phase 1 | Complete |
| COMP-02 | Phase 1 | Complete |

**Coverage:**
- v1 requirements: 22 total
- Mapped to phases: 22
- Unmapped: 0

---
*Requirements defined: 2026-01-25*
*Last updated: 2026-01-26 after Phase 4 completion*
