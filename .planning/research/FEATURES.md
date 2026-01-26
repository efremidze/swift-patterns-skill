# Feature Research

**Domain:** Swift/SwiftUI agent skill (refactor + review guidance)
**Researched:** 2026-01-25
**Confidence:** MEDIUM

## Feature Landscape

### Table Stakes (Users Expect These)

Features users assume exist. Missing these = product feels incomplete.

| Feature | Why Expected | Complexity | Notes |
|---------|--------------|------------|-------|
| SwiftUI state management guidance | State ownership and data flow are core to SwiftUI correctness | MEDIUM | Cover `@State`, `@Binding`, `@Observable`, `@Environment`, and main-actor UI updates without deep concurrency dives |
| Navigation patterns guidance | Navigation is central to SwiftUI apps | MEDIUM | Include `NavigationStack`, `navigationDestination`, and sheet/presentation patterns |
| List/collection best practices | Lists and grids are common UI primitives | MEDIUM | Stable identity, `ForEach` pitfalls, `LazyVStack`/`LazyHGrid` usage, diffing considerations |
| Refactor checklist (behavior-preserving) | Users want safe, repeatable refactors | MEDIUM | Steps to extract views, isolate state, avoid regressions |
| Review checklist for SwiftUI | Consistent review criteria improves refactor quality | LOW | Include state ownership, view identity, layout correctness, and accessibility pass |
| Performance baseline guidance | SwiftUI performance issues are common | MEDIUM | Cover cheap wins (lazy containers, avoiding heavy work in `body`) and when to consider optimization |
| Testing/DI guidance (lightweight) | Refactors often require test seams | MEDIUM | Show how to isolate logic and inject dependencies without mandating frameworks |
| Accessibility basics | Expected in production-quality SwiftUI | MEDIUM | Labels, dynamic type, hit targets, and basic VoiceOver considerations |

### Differentiators (Competitive Advantage)

Features that set the product apart. Not required, but valuable.

| Feature | Value Proposition | Complexity | Notes |
|---------|-------------------|------------|-------|
| Decision trees for state wrapper selection | Reduces incorrect state ownership and refactor churn | MEDIUM | “If data is shared → use `@Observable`/`@Environment`…”, include edge cases |
| Refactor playbooks by goal | Faster, safer improvements with minimal regressions | HIGH | Playbooks for view extraction, navigation migration, and state hoisting |
| Modern API replacement catalog | Keeps guidance current, avoids deprecated APIs | MEDIUM | Examples like `NavigationStack` over `NavigationView`, `foregroundStyle` over `foregroundColor` |
| Before/after case studies | Builds trust with concrete patterns | HIGH | Small, focused examples with rationale and tradeoffs |
| Risk assessment cues | Helps teams avoid risky refactors | MEDIUM | Signals for when to split refactor into phases or add tests first |
| Performance tuning recipes | Adds advanced value beyond table stakes | MEDIUM | Suggestions like image downsampling, lazy loading, and view identity stability |

### Anti-Features (Commonly Requested, Often Problematic)

Features that seem good but create problems.

| Feature | Why Requested | Why Problematic | Alternative |
|---------|---------------|-----------------|-------------|
| Mandated architecture (MVVM/MVC/etc.) | “Standardization” | Conflicts with project context and agent scope | Offer neutral heuristics and focus on correctness |
| Formatting/linting rules | “Consistency” | Becomes style policing, not refactor guidance | Provide readability suggestions only when needed |
| Tool-specific steps (Instruments, IDE workflows) | “Practicality” | Not usable by all agents/environments | Provide high-level guidance; mention tools only as optional |
| Generic Swift language tutorials | “Completeness” | Dilutes SwiftUI refactor focus | Focus on SwiftUI-specific patterns and pitfalls |

## Feature Dependencies

```
[SwiftUI state management guidance]
    └──requires──> [Review checklist for SwiftUI]
                       └──requires──> [Refactor checklist (behavior-preserving)]

[Decision trees for state wrapper selection] ──enhances──> [SwiftUI state management guidance]

[Modern API replacement catalog] ──enhances──> [Refactor playbooks by goal]

[Performance tuning recipes] ──requires──> [Performance baseline guidance]

[Testing/DI guidance (lightweight)] ──enhances──> [Refactor checklist (behavior-preserving)]
```

### Dependency Notes

- **SwiftUI state management guidance requires Review checklist for SwiftUI:** review criteria are derived from state ownership rules.
- **Review checklist for SwiftUI requires Refactor checklist:** reviews need a shared definition of “safe” refactors.
- **Decision trees enhance state management guidance:** decision logic operationalizes the guidance.
- **Modern API replacement catalog enhances refactor playbooks:** enables safe migrations during refactors.
- **Performance tuning recipes require baseline guidance:** avoid premature optimization without shared baselines.
- **Testing/DI guidance enhances refactor checklist:** encourages safer behavior-preserving changes.

## MVP Definition

### Launch With (v1)

Minimum viable product — what's needed to validate the concept.

- [ ] SwiftUI state management guidance — core correctness driver
- [ ] Refactor checklist (behavior-preserving) — primary user workflow
- [ ] Review checklist for SwiftUI — makes guidance actionable
- [ ] Navigation patterns guidance — common refactor pain point
- [ ] Performance baseline guidance — addresses common SwiftUI friction

### Add After Validation (v1.x)

Features to add once core is working.

- [ ] Decision trees for state wrapper selection — add when users need faster choices
- [ ] Modern API replacement catalog — add when migration questions appear

### Future Consideration (v2+)

Features to defer until product-market fit is established.

- [ ] Refactor playbooks by goal — higher cost, requires more examples
- [ ] Before/after case studies — best once canonical patterns are settled
- [ ] Performance tuning recipes — only after baseline guidance stabilizes

## Feature Prioritization Matrix

| Feature | User Value | Implementation Cost | Priority |
|---------|------------|---------------------|----------|
| SwiftUI state management guidance | HIGH | MEDIUM | P1 |
| Refactor checklist (behavior-preserving) | HIGH | MEDIUM | P1 |
| Review checklist for SwiftUI | HIGH | LOW | P1 |
| Navigation patterns guidance | HIGH | MEDIUM | P1 |
| Performance baseline guidance | MEDIUM | MEDIUM | P1 |
| Testing/DI guidance (lightweight) | MEDIUM | MEDIUM | P2 |
| Decision trees for state wrapper selection | MEDIUM | MEDIUM | P2 |
| Modern API replacement catalog | MEDIUM | MEDIUM | P2 |
| Refactor playbooks by goal | HIGH | HIGH | P3 |
| Before/after case studies | MEDIUM | HIGH | P3 |
| Performance tuning recipes | MEDIUM | MEDIUM | P3 |

**Priority key:**
- P1: Must have for launch
- P2: Should have, add when possible
- P3: Nice to have, future consideration

## Competitor Feature Analysis

| Feature | Competitor A | Competitor B | Our Approach |
|---------|--------------|--------------|--------------|
| SwiftUI state guidance | Apple SwiftUI docs (reference-driven) | Community guides (opinionated) | Agent-ready, decision-oriented guidance |
| Refactor checklist | Scattered blog advice | Ad hoc code review practices | Single, reusable checklist with rationale |
| Navigation guidance | Apple docs (API-focused) | Tutorials/examples | Migration-focused patterns with pitfalls |
| Performance guidance | Apple docs (conceptual) | Performance tips posts | Refactor-safe baseline rules + optional recipes |
| Testing/DI guidance | XCTest docs (framework-only) | Blog-specific approaches | Tool-agnostic test seam guidance |

## Sources

- https://developer.apple.com/documentation/swiftui (official reference; JS-rendered)
- https://developer.apple.com/documentation/swiftui/navigationstack (official reference; JS-rendered)
- https://developer.apple.com/documentation/observation (official reference; JS-rendered)
- https://developer.apple.com/documentation/xctest (official reference; JS-rendered)

---
*Feature research for: Swift/SwiftUI agent skill (refactor + review guidance)*
*Researched: 2026-01-25*
