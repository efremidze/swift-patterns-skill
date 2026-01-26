# Phase 3: SwiftUI Guidance Core - Research

**Researched:** 2026-01-26
**Domain:** SwiftUI core guidance (state, navigation, lists, layout, scrolling, async in views)
**Confidence:** LOW

## Summary

This research focuses on the core SwiftUI guidance needed for Phase 3: state ownership and decision trees, modern navigation and presentation, list/collection identity and lazy containers, view composition and data flow, layout/adaptive patterns, scrolling with safe pagination triggers, and lightweight async guidance with `.task` and `@MainActor`. The goal is actionable, prescriptive rules that help planners build user-facing guidance content without drifting into architecture mandates or tool-specific steps.

Apple’s SwiftUI reference documentation is JS-rendered and not fully retrievable via the available tools, so this phase relies on internal guidance and general SwiftUI practices. As a result, deprecation mappings and API behavior claims are flagged for verification. The plan should include a validation step against Apple’s SwiftUI docs before finalizing the “modern replacement” catalog.

**Primary recommendation:** Build guidance around ownership-driven state selection, value-based navigation with `NavigationStack`, stable list identity, and `.task`-based async work with `@MainActor` UI updates, then verify all modern API replacements against Apple docs.

## Standard Stack

The established libraries/tools for this domain:

### Core
| Library | Version | Purpose | Why Standard |
|---------|---------|---------|--------------|
| SwiftUI | Apple platform SDK (iOS 16+ for `NavigationStack`) | UI framework for views, navigation, layout, lists | Core framework for SwiftUI app UI and navigation. Confidence: LOW (JS-only docs). |
| Swift (Stdlib) | Apple toolchain | Language features, property wrappers, `@MainActor` | Required for UI state and concurrency annotations. Confidence: LOW (JS-only docs). |
| Swift Concurrency | Apple toolchain | `async/await`, `.task`, cancellation | Standard async model used by SwiftUI. Confidence: LOW (JS-only docs). |
| Observation (`@Observable`) | Apple platform SDK (iOS 17+) | Observable reference types for state | Recommended for new SwiftUI state models. Confidence: LOW (JS-only docs). |

### Supporting
| Library | Version | Purpose | When to Use |
|---------|---------|---------|-------------|
| None required | N/A | SwiftUI uses platform SDK | Use only standard frameworks unless project requirements add dependencies. |

### Alternatives Considered
| Instead of | Could Use | Tradeoff |
|------------|-----------|----------|
| `@Observable` | `ObservableObject` + `@Published` | Wider OS compatibility but more boilerplate. Confidence: LOW (JS-only docs). |

**Installation:**
```bash
# No packages required. SwiftUI ships with the Apple platform SDK.
```

## Architecture Patterns

### Recommended Project Structure

Optional content organization for SwiftUI guidance docs (not a required app structure):

```
swift-agent-skill/
├── SKILL.md
├── references/
│   ├── state.md                    # State wrappers + decision trees
│   ├── view-composition.md         # View composition
│   ├── navigation.md               # NavigationStack + presentation patterns
│   ├── lists-collections.md        # List identity + lazy containers
│   ├── layout.md                   # Stacks, alignment, adaptive layout
│   ├── scrolling.md                # ScrollView + pagination triggers
│   └── concurrency.md              # .task + @MainActor UI updates
│   ├── modern-swiftui-apis.md      # Deprecated + modern API catalog
└── assets/        # optional
```

### Pattern 1: Ownership-Driven State Selection
**What:** Use ownership to decide state wrappers: view-owned state uses `@State`, parent-owned state uses `@Binding`, shared app-wide state uses `@Environment` or `@Observable` model instances.
**When to use:** Any feature that introduces mutable state or data flow across view boundaries.
**Example:**
```swift
// Source: https://developer.apple.com/documentation/swiftui/
struct ParentView: View {
    @State private var count = 0

    var body: some View {
        CounterView(count: $count)
    }
}

struct CounterView: View {
    @Binding var count: Int

    var body: some View {
        Button("Count: \(count)") { count += 1 }
    }
}
```

### Pattern 2: Value-Based Navigation with `NavigationStack`
**What:** Use `NavigationStack` plus `NavigationLink(value:)` and `navigationDestination(for:)` for type-safe navigation.
**When to use:** Modern SwiftUI navigation (iOS 16+), especially for lists and detail flows.
**Example:**
```swift
// Source: https://developer.apple.com/documentation/swiftui/
enum Route: Hashable {
    case detail(Item)
}

struct ContentView: View {
    let items: [Item]

    var body: some View {
        NavigationStack {
            List(items) { item in
                NavigationLink(value: Route.detail(item)) {
                    Text(item.title)
                }
            }
            .navigationDestination(for: Route.self) { route in
                switch route {
                case .detail(let item):
                    DetailView(item: item)
                }
            }
        }
    }
}
```

### Pattern 3: Stable Identity in Lists and Collections
**What:** Ensure `ForEach` uses stable identifiers and prefer `List`/`LazyVStack` for large collections.
**When to use:** Any dynamic collection or list with insert/remove/reorder.
**Example:**
```swift
// Source: https://developer.apple.com/documentation/swiftui/
struct Item: Identifiable {
    let id: UUID
    let title: String
}

List(items) { item in
    Text(item.title)
}
```

### Pattern 4: View Composition and Data Flow
**What:** Extract subviews and pass data via immutable values, `@Binding`, or callbacks.
**When to use:** Large views, repeated UI elements, or child views that edit parent state.
**Example:**
```swift
// Source: https://developer.apple.com/documentation/swiftui/
struct EditableRow: View {
    let label: String
    @Binding var value: String

    var body: some View {
        HStack {
            Text(label)
            TextField("", text: $value)
        }
    }
}
```

### Pattern 5: Async Work with `.task` and UI Updates on `@MainActor`
**What:** Use `.task` for async work tied to view lifecycle and ensure UI mutations run on `@MainActor`.
**When to use:** Loading data when a view appears or when an ID changes.
**Example:**
```swift
// Source: https://developer.apple.com/documentation/swift/concurrency
@MainActor
final class ViewModel {
    var items: [Item] = []

    func load() async {
        items = await fetchItems()
    }
}

struct ListView: View {
    let viewModel: ViewModel

    var body: some View {
        List(viewModel.items) { item in
            Text(item.title)
        }
        .task {
            await viewModel.load()
        }
    }
}
```

### Anti-Patterns to Avoid
- **Using `.indices` for dynamic `ForEach`:** Breaks identity and can crash when data mutates; use stable IDs.
- **Nested `NavigationStack`s:** Leads to unexpected back behavior; keep one stack at the root for a flow.
- **Side effects in `body`:** Causes repeated work; move async or mutation to `.task` or `.onChange`.

## Don't Hand-Roll

Problems that look simple but have existing solutions:

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Navigation stack management | Custom routing stacks | `NavigationStack` + `navigationDestination` | Handles back stack, state restoration, and routing semantics. |
| List diffing/identity | Manual index tracking | `List`/`ForEach` with `Identifiable` | Stable identity and efficient diffing are built in. |
| View lifecycle async work | Manual task storage in views | `.task` with cancellation | `.task` is cancellation-aware and tied to view lifecycle. |
| Global shared state | Singleton globals | `@Environment` / `@Observable` model | Standard SwiftUI data flow and updates. |

**Key insight:** SwiftUI already provides lifecycle-aware and identity-aware primitives; custom implementations introduce subtle bugs and state mismatch.

## Common Pitfalls

### Pitfall 1: Incorrect State Ownership
**What goes wrong:** Using `@State` for shared or long-lived state causes resets and out-of-sync UI.
**Why it happens:** Ownership rules are unclear.
**How to avoid:** Use `@State` only for view-owned values; pass `@Binding` to children; use `@Observable` or `@Environment` for shared state.
**Warning signs:** State resets on navigation or view refresh; duplicate sources of truth.

### Pitfall 2: Unstable Identity in `ForEach`
**What goes wrong:** Using indices or non-unique IDs causes incorrect updates or crashes.
**Why it happens:** Collections mutate and indices change.
**How to avoid:** Adopt `Identifiable` or supply stable `id:`.
**Warning signs:** Rows display the wrong data after insert/delete/reorder.

### Pitfall 3: Deprecated/Legacy Navigation APIs
**What goes wrong:** Guidance recommends `NavigationView` or old navigation bar APIs.
**Why it happens:** Legacy docs and samples persist.
**How to avoid:** Prefer `NavigationStack`, `navigationDestination`, and `toolbar`.
**Warning signs:** Use of `NavigationView`, `navigationBarItems`, or `navigationBarTitle` as defaults.

### Pitfall 4: Async Work Not Tied to View Lifecycle
**What goes wrong:** Using `onAppear` or `Task {}` without cancellation causes duplicate work or stale updates.
**Why it happens:** Missing `.task` guidance.
**How to avoid:** Prefer `.task(id:)` for lifecycle-bound async and check cancellation in long-running work.
**Warning signs:** Multiple network requests on navigation or scrolling.

### Pitfall 5: UI Mutations Off Main Actor
**What goes wrong:** UI state updates from background tasks can crash or glitch.
**Why it happens:** No `@MainActor` guidance.
**How to avoid:** Mark UI-facing models or update calls with `@MainActor`.
**Warning signs:** Random UI inconsistencies or concurrency warnings.

## Code Examples

Verified patterns from official sources (URLs are JS-rendered; verify content before finalizing):

### State Ownership + Binding
```swift
// Source: https://developer.apple.com/documentation/swiftui/
struct Parent: View {
    @State private var isOn = false

    var body: some View {
        ToggleRow(isOn: $isOn)
    }
}

struct ToggleRow: View {
    @Binding var isOn: Bool

    var body: some View {
        Toggle("Enabled", isOn: $isOn)
    }
}
```

### List with Stable Identity
```swift
// Source: https://developer.apple.com/documentation/swiftui/
struct Item: Identifiable {
    let id: UUID
    let title: String
}

List(items) { item in
    Text(item.title)
}
```

### Navigation Destination
```swift
// Source: https://developer.apple.com/documentation/swiftui/
enum Route: Hashable { case detail(Item) }

NavigationStack {
    NavigationLink(value: Route.detail(item)) { Text(item.title) }
}
.navigationDestination(for: Route.self) { route in
    if case .detail(let item) = route {
        DetailView(item: item)
    }
}
```

### `.task` for Async Load
```swift
// Source: https://developer.apple.com/documentation/swift/concurrency
struct DetailView: View {
    @State private var item: Item?
    let id: Item.ID

    var body: some View {
        Group {
            if let item { Text(item.title) } else { ProgressView() }
        }
        .task(id: id) {
            item = await fetchItem(id)
        }
    }
}
```

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| `NavigationView` | `NavigationStack` | iOS 16 | Modern navigation with value-based destinations. |
| `navigationBarItems` | `toolbar` | iOS 14+ | Unified toolbar API. |
| `navigationBarTitle` | `navigationTitle` | iOS 14+ | Consistent title configuration. |
| `foregroundColor` | `foregroundStyle` | iOS 15+ | Modern styling API; supports more styles. |

**Deprecated/outdated:**
- `NavigationView`: replaced by `NavigationStack` for modern navigation.
- `navigationBarItems`: replaced by `toolbar`.
- `navigationBarTitle`: replaced by `navigationTitle`.

## Open Questions

1. **Which SwiftUI APIs are currently deprecated vs merely superseded?**
   - What we know: Common replacements include `NavigationStack`, `toolbar`, `navigationTitle`, and `foregroundStyle`.
   - What's unclear: Exact deprecation annotations and OS availability due to JS-only docs.
   - Recommendation: Validate against Apple’s SwiftUI docs before finalizing MOD-01 catalog.

2. **Observation framework availability details**
   - What we know: `@Observable` is modern guidance for state models.
   - What's unclear: Precise OS availability and any migration caveats.
   - Recommendation: Confirm availability in official docs and note minimum OS versions.

## Sources

### Primary (HIGH confidence)
- https://developer.apple.com/documentation/swiftui/ - SwiftUI API reference (JS-rendered)
- https://developer.apple.com/documentation/swift/ - Swift language reference (JS-rendered)
- https://developer.apple.com/documentation/swift/concurrency - Swift concurrency reference (JS-rendered)

### Secondary (MEDIUM confidence)
- None (JS-rendered docs were not fully retrievable with current tools)

### Tertiary (LOW confidence)
- Internal SwiftUI guidance notes under `swift-agent-skill/references/` (non-authoritative, for synthesis only)

## Metadata

**Confidence breakdown:**
- Standard stack: LOW - Apple docs are JS-only; versions/availability need verification.
- Architecture: LOW - patterns based on internal guidance; no official access.
- Pitfalls: MEDIUM - consistent with existing internal guidance, but needs validation.

**Research date:** 2026-01-26
**Valid until:** 2026-02-25
