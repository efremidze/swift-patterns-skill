# Code Review & Refactoring (SwiftUI)

**Required references:**
- **Constraints (mandatory):** See [SKILL.md Constraints](../SKILL.md#constraints).

## Overview
Use this reference to review SwiftUI code for quality risks and refactor safely while preserving behavior. Focus on state ownership, data flow, navigation, identity stability, error handling, and testability. Keep guidance aligned with the shared Constraints.

## Scope and Intent

**Review:** Provide findings and risks without changing code.

**Refactor:** Change code while preserving existing behavior.

If intent is unclear, ask a direct question before proceeding.

## Review Focus Areas
- State ownership matches intent (`@State` local, `@Binding` parent-owned, `@Observable` shared).
- Unidirectional data flow (data down, events up) with a single source of truth.
- Navigation state lives in one place and is not duplicated across views.
- Stable identity for lists and `ForEach` (no `.indices` for dynamic data).
- Async work is tied to view lifecycle and cancellation is respected.
- Error handling is explicit and user-facing paths are safe.
- Testability is preserved (logic separated enough to exercise without UI).

## Refactor Focus Areas
- Capture current behavior and guard it during changes.
- Preserve identity, state ownership, and navigation source of truth.
- Keep data flow direction intact when extracting views or helpers.
- Maintain async lifecycle boundaries when moving work across views.
- Avoid introducing architecture mandates or unrelated abstractions.

## SwiftUI Code Smells
- Two sources of truth for the same state (duplicate `@State` and model state).
- Derived state stored instead of computed (drift between values).
- Side effects in `body` or view initializers.
- `ForEach` using unstable identity or positional indices.
- Navigation driven by multiple paths or competing stacks.
- Work in `body` that should be precomputed or moved to a model.
- Error paths that silently fail or swallow failures.

## Review Checklist
- [ ] Confirm review intent and capture scope.
- [ ] Validate state ownership and bindings.
- [ ] Confirm data flow direction and single source of truth.
- [ ] Check list identity and selection persistence.
- [ ] Verify navigation state is centralized.
- [ ] Ensure async work is lifecycle-aware.
- [ ] Verify error handling and edge cases.
- [ ] Note test coverage risks when behavior is critical.

## Refactor Checklist
- [ ] Capture behavior baseline and expected invariants.
- [ ] Preserve identity and selection state in lists.
- [ ] Keep state ownership mapping consistent.
- [ ] Maintain a single navigation source of truth.
- [ ] Keep async work cancellable and tied to lifecycle.
- [ ] Avoid introducing architecture mandates.
- [ ] Re-check behavior after each change.

## Example: Duplicate Source of Truth
```swift
// Before: state duplicated between view and model
struct PlayerView: View {
    @State private var isPlaying = false
    let model: PlayerModel

    var body: some View {
        Toggle("Playing", isOn: $isPlaying)
            .onChange(of: isPlaying) { model.isPlaying = $0 }
    }
}

// After: single source of truth via binding
struct PlayerView: View {
    @Binding var isPlaying: Bool

    var body: some View {
        Toggle("Playing", isOn: $isPlaying)
    }
}
```

## Example: Unstable List Identity
```swift
// Before: unstable identity for dynamic content
ForEach(items.indices, id: \ .self) { index in
    RowView(item: items[index])
}

// After: stable identity from the model
ForEach(items, id: \ .id) { item in
    RowView(item: item)
}
```

## Anti-Patterns to Avoid
- Global mutable state used by multiple views without ownership.
- Mixed navigation sources (e.g., multiple stacks or competing paths).
- Copying view state into models without clear ownership boundaries.
- Silent error handling that hides failures from users.
- Over-abstraction that reduces testability without clear payoff.
