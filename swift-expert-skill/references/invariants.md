# Refactor Invariants (SwiftUI)

These invariants must hold when refactoring SwiftUI code. They are correctness and behavior-preservation requirements, not architectural mandates.

**Constraints are mandatory:** See [SKILL.md Constraints](../SKILL.md#constraints).

## Invariants

- **Stable identity in lists:** Use stable identifiers for `List`/`ForEach` content. Do not use `.indices` for dynamic data.
- **State ownership mapping:** `@State` for view-local state, `@Binding` for parent-owned state, `@Observable` for shared state.
- **Single navigation source of truth:** Keep one `NavigationStack` root and a single path/source; avoid competing roots or duplicated path state.
- **Cancellable async work:** Tie async work to view lifecycle with `.task` and cancel outstanding work when inputs change.
- **Unidirectional data flow:** Data flows down the view hierarchy; events flow up via bindings or callbacks (no direct child mutation of parent state).
