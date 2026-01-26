# Phase 4: Quality + Playbooks - Research

**Researched:** 2026-01-26
**Domain:** SwiftUI quality/performance guidance, testing/DI seams, refactor playbooks
**Confidence:** LOW

## Summary

This research covers SwiftUI performance guidance for common pitfalls and safe optimizations, identity stability and expensive work avoidance, lightweight testing/DI seams for refactor safety, and goal-based refactor playbooks (view extraction, navigation migration, state hoisting). The focus is on prescriptive, behavior-preserving guidance that aligns with the Swift skill constraints and avoids tool mandates or architecture prescriptions.

Official SwiftUI documentation is JS-rendered and not fully retrievable with available tools, so the findings rely on internal guidance sources and known SwiftUI practices. All API-availability or deprecation details must be verified against Apple documentation before final guidance is finalized.

**Primary recommendation:** Build Phase 4 guidance around a concise performance baseline checklist, identity-stability and expensive-work avoidance patterns, protocol-based DI seams for refactor safety, and step-by-step playbooks anchored to refactor invariants.

## Standard Stack

The established libraries/tools for this domain:

### Core
| Library | Version | Purpose | Why Standard |
|---------|---------|---------|--------------|
| SwiftUI | Apple platform SDK | UI framework, lists, navigation, view lifecycle | Core framework for SwiftUI performance and identity patterns. Confidence: LOW (JS-only docs). |
| Swift (Stdlib) | Apple toolchain | Language features and property wrappers | Required for state and identity guidance. Confidence: LOW (JS-only docs). |
| Swift Concurrency | Apple toolchain | `.task`, cancellation, background work | Standard async model used for expensive work avoidance. Confidence: LOW (JS-only docs). |
| XCTest | Apple platform SDK | Unit testing baseline | Default test framework for Swift. Confidence: LOW (JS-only docs). |

### Supporting
| Library | Version | Purpose | When to Use |
|---------|---------|---------|-------------|
| None required | N/A | SwiftUI uses platform SDK | Use only standard frameworks unless project requirements add dependencies. |

### Alternatives Considered
| Instead of | Could Use | Tradeoff |
|------------|-----------|----------|
| Manual DI seams | DI frameworks (Swinject, etc.) | Less boilerplate but adds dependency and learning curve. Keep Phase 4 framework-free. |
| XCTest-only view assertions | ViewInspector (third-party) | Enables SwiftUI view inspection but adds dependency; optional, not required. |

**Installation:**
```bash
# No packages required. SwiftUI and XCTest ship with the Apple platform SDK.
```

## Architecture Patterns

### Recommended Project Structure

Optional content organization for SwiftUI guidance docs (not a required app structure):

```
swift-patterns/
├── SKILL.md
├── references/
│   ├── performance.md              # Performance baseline + safe optimizations
│   ├── testing-di.md               # Lightweight DI and test seams
│   ├── workflows-refactor.md       # Refactor invariants and checklist
│   └── code-review-refactoring.md  # Refactor patterns and risks
└── assets/                         # optional
```

### Pattern 1: Performance Baseline Checklist (SwiftUI)
**What:** Start with a short checklist to identify common SwiftUI pitfalls and safe optimizations.
**When to use:** Any performance guidance, especially before recommending changes.
**Example:**
```swift
// Source: swift-patterns/references/performance.md
// Baseline checks
// - Avoid expensive computations in body
// - Use LazyVStack/LazyHStack for long lists
// - Tie async work to view lifecycle with .task
// - Avoid blocking main thread with heavy work
```

### Pattern 2: Identity Stability + Expensive Work Avoidance
**What:** Use stable IDs for lists and move expensive work out of `body`.
**When to use:** Any dynamic list or view with heavy computation/formatting.
**Example:**
```swift
// Source: swift-patterns/references/performance.md
// Bad: creates formatter every render
var body: some View {
    let formatter = DateFormatter()
    return Text(formatter.string(from: date))
}

// Good: create once
private let formatter: DateFormatter = {
    let f = DateFormatter()
    f.dateStyle = .medium
    return f
}()
```

### Pattern 3: Lightweight DI Seams for Refactor Safety
**What:** Use protocol abstractions and initializer injection for testable seams.
**When to use:** Refactors that touch dependencies or side effects.
**Example:**
```swift
// Source: swift-patterns/references/testing-di.md
protocol UserServiceProtocol {
    func fetchUser(id: String) async throws -> User
}

class UserViewModel {
    private let userService: UserServiceProtocol

    init(userService: UserServiceProtocol) {
        self.userService = userService
    }
}
```

### Pattern 4: Goal-Based Refactor Playbooks
**What:** Provide step-by-step playbooks aligned to invariants.
**When to use:** View extraction, navigation migration, or state hoisting.
**Example:**
```swift
// Source: swift-patterns/references/workflows-refactor.md
// Playbook steps (high level):
// 1) Capture current behavior
// 2) Preserve stable identity and state ownership
// 3) Keep one navigation source of truth
// 4) Ensure async work remains cancellable and tied to .task
// 5) Re-check invariants after changes
```

### Anti-Patterns to Avoid
- **Unstable identity (`.indices`) in dynamic `ForEach`:** Leads to wrong updates or crashes; use stable IDs.
- **Creating objects in `body`:** Recreates on every render; move to stored properties or memoize.
- **Singleton dependencies for refactors:** Blocks test seams; use protocol-based DI.
- **Refactor without behavior baseline:** Increases regressions; capture before/after behavior.

## Don't Hand-Roll

Problems that look simple but have existing solutions:

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| List diffing/identity | Manual index tracking | `List`/`ForEach` with stable `id` | SwiftUI handles diffing and identity correctly. |
| Navigation stack | Custom routing stacks | `NavigationStack` + `navigationDestination` | Built-in back stack semantics and state restoration. |
| Ad-hoc async lifecycle | `Task {}` in `body` | `.task(id:)` | Lifecycle-aware cancellation reduces duplicate work. |
| Test seams | Global singletons | Protocol + initializer injection | Enables lightweight tests and safe refactors. |

**Key insight:** SwiftUI provides identity, navigation, and lifecycle primitives; custom solutions increase bugs and refactor risk.

## Common Pitfalls

### Pitfall 1: Expensive Work in `body`
**What goes wrong:** Frequent recomputation and object creation hurt frame time.
**Why it happens:** `body` recomputes often; work is repeated.
**How to avoid:** Move work to stored properties, memoize, or compute once per input.
**Warning signs:** Jank during scrolling or frequent re-rendering.

### Pitfall 2: Unstable List Identity
**What goes wrong:** Rows update incorrectly or crash on insert/delete/reorder.
**Why it happens:** `.indices` or non-unique IDs change as data mutates.
**How to avoid:** Adopt `Identifiable` or supply stable `id:`.
**Warning signs:** Data mismatches after collection mutations.

### Pitfall 3: Async Work Detached from View Lifecycle
**What goes wrong:** Duplicate or stale work continues after navigation.
**Why it happens:** Using `Task {}` or `onAppear` without cancellation.
**How to avoid:** Use `.task(id:)` and check cancellation in long-running work.
**Warning signs:** Multiple network calls on navigation/scroll.

### Pitfall 4: No DI Seams During Refactor
**What goes wrong:** Refactors become risky because behavior can’t be isolated or tested.
**Why it happens:** Direct instantiation and singletons are hard to substitute.
**How to avoid:** Introduce protocols and initializer injection where behavior changes.
**Warning signs:** Changes require touching multiple files or environments to test.

### Pitfall 5: Refactor Breaks Invariants
**What goes wrong:** Behavior changes in navigation, identity, or state ownership.
**Why it happens:** Refactor steps ignore invariants.
**How to avoid:** Check invariants before/after; split refactors when risk cues appear.
**Warning signs:** Navigation stack resets, state resets, or missing updates.

## Code Examples

Verified patterns from internal references (needs validation against Apple docs):

### Stable Identity in Lists
```swift
// Source: swift-patterns/references/invariants.md
struct Item: Identifiable {
    let id: UUID
    let title: String
}

List(items) { item in
    Text(item.title)
}
```

### Avoid Expensive Work in `body`
```swift
// Source: swift-patterns/references/performance.md
private let formatter: DateFormatter = {
    let f = DateFormatter()
    f.dateStyle = .medium
    return f
}()

var body: some View {
    Text(formatter.string(from: date))
}
```

### Lightweight DI Seam
```swift
// Source: swift-patterns/references/testing-di.md
protocol NetworkManagerProtocol {
    func request() async throws -> Data
}

class MyService {
    private let networkManager: NetworkManagerProtocol

    init(networkManager: NetworkManagerProtocol) {
        self.networkManager = networkManager
    }
}
```

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| `VStack` for long lists | `LazyVStack`/`LazyHStack` | SwiftUI introduction | Reduces memory and improves scroll performance. |
| Manual async lifecycle | `.task(id:)` | SwiftUI 3+ | Ties work to view lifecycle with cancellation. |
| Singleton dependencies | Protocol + initializer injection | Standard testing practice | Enables lightweight tests and safer refactors. |

**Deprecated/outdated:**
- Custom list diffing or index-based identity for dynamic lists; use stable IDs.

## Open Questions

1. **Which performance-specific SwiftUI APIs are officially recommended or deprecated?**
   - What we know: SwiftUI provides list and lifecycle primitives; internal guidance suggests Lazy stacks and `.task`.
   - What's unclear: Exact deprecations and availability due to JS-only docs.
   - Recommendation: Validate against Apple’s SwiftUI docs before final guidance.

2. **Observation framework availability details**
   - What we know: `@Observable` is recommended in prior phases for new code.
   - What's unclear: Precise OS availability and any testing/DI implications.
   - Recommendation: Confirm availability and document minimum OS versions.

## Sources

### Primary (HIGH confidence)
- None (official SwiftUI docs were not fully retrievable).

### Secondary (MEDIUM confidence)
- None.

### Tertiary (LOW confidence)
- swift-patterns/references/performance.md
- swift-patterns/references/testing-di.md
- swift-patterns/references/workflows-refactor.md
- swift-patterns/references/invariants.md

## Metadata

**Confidence breakdown:**
- Standard stack: LOW - Apple docs are JS-only; versions/availability need verification.
- Architecture: LOW - patterns based on internal guidance; no official access.
- Pitfalls: MEDIUM - consistent with internal guidance, but needs validation.

**Research date:** 2026-01-26
**Valid until:** 2026-02-25
