# View Composition

Extract views to improve readability without breaking identity or state ownership. Data flows down, events flow up.

## Safe Extraction Patterns

- Keep state in the view that owns the lifetime.
- Pass immutable values for display-only content.
- Use `@Binding` for edit flows.
- Use callbacks for events (tap, submit, delete).

## Parent/Child Data Flow

- **Display-only:** pass values as plain properties.
- **Editable:** pass a `Binding`.
- **Actions:** pass closures for intent events.

## Smells and Fixes

- **Smell:** Child view owns state that should be shared.
  - **Fix:** Hoist state to the parent and pass a `Binding`.
- **Smell:** Multiple siblings compute the same derived value.
  - **Fix:** Compute once in the parent and pass the result.
- **Smell:** Child mutates parent state by reaching into environment or global state.
  - **Fix:** Pass explicit bindings or callbacks.

## Bad -> Better

### Mutating shared state in the child
```swift
// Bad: child owns shared state
struct ToggleRow: View {
    @State private var isOn = false
    var body: some View { Toggle("Enabled", isOn: $isOn) }
}

// Better: parent owns, child edits
struct ToggleRow: View {
    @Binding var isOn: Bool
    var body: some View { Toggle("Enabled", isOn: $isOn) }
}
```

### Overloading a child with business logic
```swift
// Bad: child decides deletion rules
struct Row: View {
    let item: Item
    var body: some View {
        Button("Delete") { item.deleteIfAllowed() }
    }
}

// Better: child exposes intent, parent decides
struct Row: View {
    let item: Item
    let onDelete: () -> Void

    var body: some View {
        Button("Delete", action: onDelete)
    }
}
```

## Where State Should Live

- **Local state:** view-specific toggles, text input, transient UI.
- **Hoisted state:** values shared across multiple siblings.
- **Shared state:** environment-injected models used across screens.

## Invariants

- Stable IDs stay stable across updates.
- State ownership stays with the correct view.
- Navigation state remains the source of truth.
- Async work remains cancellable and tied to view lifecycle.
