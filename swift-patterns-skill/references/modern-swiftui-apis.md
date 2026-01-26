# Modern SwiftUI API Replacements

Use modern APIs when targeting the latest OS versions. This catalog focuses on common legacy patterns and their preferred replacements.

## Replacement Catalog

| Legacy / Older API | Prefer | Notes |
| --- | --- | --- |
| `NavigationView` | `NavigationStack` | Modern value-based navigation (iOS 16+).
| `NavigationLink(destination:)` | `NavigationLink(value:)` + `navigationDestination(for:)` | Prefer value-based destinations for type safety (iOS 16+).
| `navigationBarTitle(_:)` | `navigationTitle(_:)` | Unified title API.
| `navigationBarItems(leading:trailing:)` | `toolbar` | Consistent toolbar placement and roles.
| `foregroundColor(_:)` | `foregroundStyle(_:)` | Supports materials and gradients (iOS 15+).
| `accentColor(_:)` | `tint(_:)` | Use `tint` for accent styling (iOS 15+).
| `onReceive(NotificationCenter)` | `task` or `onChange` | Prefer lifecycle-safe updates when feasible.

## Notes

- Prefer modern APIs when the deployment target allows.
- For mixed OS support, use availability checks to keep behavior consistent.
- Avoid claiming deprecation unless confirmed; this list focuses on preferred usage.
