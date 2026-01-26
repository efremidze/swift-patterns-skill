# Phase 02: Decisioned Workflows + Safety - Research

**Researched:** 2026-01-25
**Domain:** SwiftUI refactor/review workflow routing and safety checklists for an Agent Skills package
**Confidence:** MEDIUM

## Summary

This phase focuses on decisioned workflows (review vs refactor) and safety checklists that preserve SwiftUI behavior. Research was grounded in the Agent Skills specification for structure and in existing internal SwiftUI reference guidance for state ownership, navigation, and review/refactor patterns. Official SwiftUI docs could not be fetched due to JS-only pages, so SwiftUI-specific findings rely on internal references and must be validated against Apple docs before final release.

The standard approach is to use a single decision gate for routing (intent cues), then apply a shared constraints section plus workflow-specific checklists. A behavior-preserving refactor checklist should emphasize identity stability, state ownership, navigation source of truth, and cancellable async work. The review checklist should be consistent, use a small taxonomy of findings, and reference the same invariants to avoid drift.

**Primary recommendation:** Implement one routing decision table and a shared constraints + invariants section referenced by both refactor and review workflows.

## Standard Stack

The established libraries/tools for this domain:

### Core
| Library | Version | Purpose | Why Standard |
|---------|---------|---------|--------------|
| Agent Skills Open Format | Current spec | Skill packaging and interoperability | Required format for skill distribution and validation |
| Markdown + YAML frontmatter | N/A | Authoring skill instructions | Mandated by the spec for SKILL.md |
| SwiftUI (Apple) | iOS 16+ for NavigationStack (internal reference) | Domain API surface for guidance | Core framework for SwiftUI refactor/review rules |
| Observation (@Observable) | iOS 17+ (internal reference) | Modern state observation | Preferred over ObservableObject for new code |

### Supporting
| Library | Version | Purpose | When to Use |
|---------|---------|---------|-------------|
| Skill reference files in `references/` | N/A | Modular guidance units | When workflows need to link to canonical rules |

### Alternatives Considered
| Instead of | Could Use | Tradeoff |
|------------|-----------|----------|
| Modular references + workflows | Single monolithic SKILL.md | Simpler, but higher drift risk and harder updates |

**Installation:**
```bash
# No packages required; Markdown/YAML skill content only.
```

## Architecture Patterns

### Recommended Project Structure
```
swift-expert-skill/
├── SKILL.md                 # Entry point (frontmatter + overview)
└── references/
    ├── decisions.md         # Review vs refactor routing
    ├── workflows-review.md  # Review checklist and output pattern
    ├── workflows-refactor.md# Refactor checklist and safety gates
    └── swiftui-*.md          # Canonical SwiftUI rules (state, nav, lists)
```

### Pattern 1: Decision-Gated Workflow Routing
**What:** Route requests to review vs refactor using explicit intent cues, then apply the corresponding checklist.
**When to use:** Any request that could be either a review or a refactor.
**Example:**
```markdown
// Source: swift-expert-skill/references/code-review-refactoring.md
Gate 1: Does the user want findings (review) or changes (refactor)?
Gate 2: Is the request SwiftUI-specific (state, navigation, lists, performance)?
Gate 3: Is the request safe to change without tests (risk cues)?
```

### Pattern 2: Shared Constraints + Invariants Section
**What:** Maintain a single constraints/invariants block that both workflows reference (no architecture mandates, stable IDs, state ownership, navigation source of truth, cancellable async work).
**When to use:** Always; it prevents drift between review and refactor workflows.
**Example:**
```markdown
// Source: AGENTS.md
- Always provide stable identity for ForEach; never use .indices for dynamic data.
- Use NavigationStack instead of deprecated NavigationView.
- Prefer @Observable for new code (iOS 17+).
```

### Pattern 3: Findings Taxonomy for Reviews
**What:** Classify findings so review output is consistent (correctness, data flow, navigation, identity, performance, accessibility).
**When to use:** Review workflow output.
**Example:**
```markdown
// Source: .planning/research/ARCHITECTURE.md
Finding categories: correctness, maintainability, performance, accessibility.
```

### Anti-Patterns to Avoid
- **Workflow-only rules:** duplicating rules inside workflows causes drift; keep rules in shared references.
- **Architecture mandates:** avoid requiring MVVM/coordinators; focus on correctness and invariants.

## Don't Hand-Roll

Problems that look simple but have existing solutions:

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Workflow routing | Ad hoc intent inference per response | Single decision table with intent cues | Consistency and auditability |
| Review/refactor safety rules | Separate, divergent checklists | Shared constraints/invariants block | Prevents contradictory guidance |
| Navigation system | Custom routing patterns in guidance | SwiftUI NavigationStack + navigationDestination | Built-in behavior and user expectations |
| State observation | Custom publish/subscribe rules | @Observable (or ObservableObject) guidance | Standard SwiftUI data flow patterns |

**Key insight:** Centralized decision logic and shared invariants reduce drift and refactor regressions.

## Common Pitfalls

### Pitfall 1: Misrouting Review vs Refactor
**What goes wrong:** Review requests get refactor advice (or vice versa).
**Why it happens:** Missing or inconsistent intent cues.
**How to avoid:** Use a single decision gate with explicit cues ("review", "find issues" vs "refactor", "extract", "simplify").
**Warning signs:** Output contains code changes when the user asked for findings only.

### Pitfall 2: State Ownership Shifts During Refactor
**What goes wrong:** State moves between views or wrappers without intent, changing behavior.
**Why it happens:** Refactors that extract views without preserving ownership rules.
**How to avoid:** Verify ownership mapping (`@State` local, `@Binding` for parent-owned, `@Observable` for shared).
**Warning signs:** New `@State` added where a binding previously existed.

### Pitfall 3: Unstable Identity in Lists
**What goes wrong:** List updates animate incorrectly or lose state.
**Why it happens:** Using `.indices` or non-stable identifiers in `ForEach`.
**How to avoid:** Require stable IDs (Identifiable or explicit `id:`).
**Warning signs:** Row state resets or reorder glitches after refactor.

### Pitfall 4: Split Navigation Source of Truth
**What goes wrong:** Back navigation or deep links behave inconsistently.
**Why it happens:** Navigation state spread across multiple views.
**How to avoid:** Keep a single navigation source of truth (e.g., one path binding).
**Warning signs:** Multiple `NavigationStack` roots or redundant path state.

### Pitfall 5: Async Work Not Cancellable
**What goes wrong:** Background tasks continue after view dismissal.
**Why it happens:** Refactors remove or bypass task cancellation points.
**How to avoid:** Tie async work to view lifecycle with `.task` and cancel outstanding tasks when input changes.
**Warning signs:** Network activity continues after navigation away.

## Code Examples

Verified patterns from internal references:

### State Ownership via @State and @Binding
```swift
// Source: swift-expert-skill/references/state.md
struct ParentView: View {
    @State private var count = 0

    var body: some View {
        ChildView(count: $count)
    }
}

struct ChildView: View {
    @Binding var count: Int

    var body: some View {
        Button("Increment") { count += 1 }
    }
}
```

### NavigationStack with Route Enum
```swift
// Source: swift-expert-skill/references/navigation.md
enum Route: Hashable {
    case settings
    case profile(username: String)
}

struct RootView: View {
    @State private var path = NavigationPath()

    var body: some View {
        NavigationStack(path: $path) {
            List {
                NavigationLink(value: Route.settings) {
                    Text("Settings")
                }
            }
            .navigationDestination(for: Route.self) { route in
                switch route {
                case .settings:
                    SettingsView()
                case .profile(let username):
                    ProfileView(username: username)
                }
            }
        }
    }
}
```

### Cancellable Async Work Pattern
```swift
// Source: swift-expert-skill/references/performance.md
struct SearchView: View {
    @State private var query = ""
    @State private var results: [String] = []
    @State private var searchTask: Task<Void, Never>?

    var body: some View {
        TextField("Search", text: $query)
            .onChange(of: query) { newValue in
                searchTask?.cancel()
                searchTask = Task {
                    try? await Task.sleep(nanoseconds: 300_000_000)
                    guard !Task.isCancelled else { return }
                    results = await performSearch(newValue)
                }
            }
    }

    private func performSearch(_ query: String) async -> [String] { [] }
}
```

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| NavigationView | NavigationStack | iOS 16+ (internal reference) | Modern navigation and value-based destinations |
| ObservableObject + @Published | @Observable | iOS 17+ (internal reference) | Less boilerplate and modern observation |
| foregroundColor | foregroundStyle | SwiftUI modern API guidance | Aligns with current SwiftUI styling APIs |

**Deprecated/outdated:**
- `NavigationView`: deprecated; replace with `NavigationStack`.

## Open Questions

1. **SwiftUI API availability and deprecations**
   - What we know: Internal references indicate NavigationStack (iOS 16+) and @Observable (iOS 17+).
   - What's unclear: Exact availability and deprecation notes from Apple docs (JS-only).
   - Recommendation: Validate against `https://developer.apple.com/documentation/swiftui/` before finalizing.

2. **ForEach identity rules source**
   - What we know: Project constraints require stable identity and ban `.indices` for dynamic content.
   - What's unclear: Official SwiftUI doc wording for identity guarantees.
   - Recommendation: Confirm in Apple docs and update invariants list accordingly.

## Sources

### Primary (HIGH confidence)
- https://agentskills.io/specification.md - Skill format and structure requirements

### Secondary (MEDIUM confidence)
- .planning/research/FEATURES.md - Phase 2 feature expectations and risk cues
- swift-expert-skill/references/state.md - State ownership and data flow guidance
- swift-expert-skill/references/navigation.md - NavigationStack and routing patterns
- swift-expert-skill/references/performance.md - Cancellable async work example
- swift-expert-skill/references/code-review-refactoring.md - Review/refactor workflow cues
- AGENTS.md - Project constraints (stable identity, modern APIs)

### Tertiary (LOW confidence)
- .planning/research/ARCHITECTURE.md - Decision-gated workflow structure (internal synthesis)
- https://developer.apple.com/documentation/swiftui/ - Official SwiftUI reference (JS-rendered; not fetched)
- https://developer.apple.com/documentation/observation - Observation reference (JS-rendered; not fetched)

## Metadata

**Confidence breakdown:**
- Standard stack: MEDIUM - Spec verified; SwiftUI API availability not verified due to JS-only docs.
- Architecture: LOW - Based on internal synthesis and needs validation.
- Pitfalls: MEDIUM - Derived from internal constraints and references.

**Research date:** 2026-01-25
**Valid until:** 2026-02-24
