---
name: swift-patterns
description: Review, refactor, or build SwiftUI features with correct state management, modern API usage, optimal view composition, navigation patterns, performance optimization, and testing best practices.
---

# swift-patterns

Review, refactor, or build SwiftUI features with correct state management, modern API usage, optimal view composition, and performance-conscious patterns. Prioritize native APIs, Apple design guidance, and testable code structure. This skill focuses on facts and best practices without enforcing specific architectural patterns.

## Workflow Decision Tree

### 1) Review existing SwiftUI code
→ Read `references/workflows-review.md` for review methodology
- Check state management against property wrapper selection (see `references/state.md`)
- Verify view composition and extraction patterns (see `references/view-composition.md`)
- Audit list performance and identity stability (see `references/lists-collections.md`)
- Validate modern API usage (see `references/modern-swiftui-apis.md`)
- Check async work patterns with .task (see `references/concurrency.md`)
- Verify navigation implementation (see `references/navigation.md`)
- Use Review Response Template (below)

### 2) Refactor existing SwiftUI code
→ Read `references/workflows-refactor.md` for refactor methodology
- Extract complex views using playbooks (see `references/refactor-playbooks.md`)
- Migrate deprecated APIs to modern equivalents (see `references/modern-swiftui-apis.md`)
- Optimize performance hot paths (see `references/performance.md`)
- Restructure state ownership (see `references/state.md`)
- Apply common patterns (see `references/patterns.md`)
- Use Refactor Response Template (below)

### 3) Implement new SwiftUI features
- Design data flow first: identify owned vs injected state (see `references/state.md`)
- Structure views for optimal composition (see `references/view-composition.md`)
- Use modern APIs only (see `references/modern-swiftui-apis.md`)
- Handle async work with .task modifier (see `references/concurrency.md`)
- Apply performance patterns early (see `references/performance.md`)
- Implement navigation flows (see `references/navigation.md`)

### 4) Answer best practice questions
- Load relevant reference file(s) based on topic (see Quick Reference Guide below)
- Provide direct guidance with examples

**If intent unclear, ask:** "Do you want findings only (review), or should I change the code (refactor)?"

## Response Templates

**Review Response:**
1. **Scope** - What was reviewed
2. **Findings** - Grouped by severity with actionable statements
3. **Evidence** - File paths or code locations
4. **Risks and tradeoffs** - What could break or needs attention
5. **Next steps** - What to fix first or verify

**Refactor Response:**
1. **Intent + scope** - What is being changed and why
2. **Changes** - Bulleted list with file paths
3. **Behavior preservation** - What should remain unchanged
4. **Next steps** - Tests or verification needed

## Quick Reference: Property Wrapper Selection

| Wrapper | Use When | Ownership |
|---------|----------|-----------|
| `@State` | Internal view state (value type or `@Observable` class) | View owns |
| `@Binding` | Child needs to modify parent's state | Parent owns |
| `@Bindable` | Injected `@Observable` object needing bindings | Injected |
| `let` | Read-only value from parent | Injected |
| `var` + `.onChange()` | Read-only value needing reactive updates | Injected |

**Key rules:**
- Always mark `@State` as `private` (makes ownership explicit)
- Never use `@State` for passed values (accepts initial value only)
- Use `@State` with `@Observable` classes (not `@StateObject`)

See `references/state.md` for detailed guidance and tradeoffs.

## Quick Reference: Modern API Replacements

| Deprecated | Modern Alternative | Notes |
|------------|-------------------|-------|
| `foregroundColor()` | `foregroundStyle()` | Supports dynamic type |
| `cornerRadius()` | `.clipShape(.rect(cornerRadius:))` | More flexible |
| `NavigationView` | `NavigationStack` | Type-safe navigation |
| `tabItem()` | `Tab` API | iOS 18+ |
| `onTapGesture()` | `Button` | Unless need location/count |
| `onChange(of:) { value in }` | `onChange(of:) { old, new in }` or `onChange(of:) { }` | Two or zero parameters |
| `UIScreen.main.bounds` | `GeometryReader` or layout APIs | Avoid hard-coded sizes |

See `references/modern-swiftui-apis.md` for complete migration guide.

## Review Checklist

Use this when reviewing SwiftUI code:

### State Management
- [ ] `@State` properties marked `private`
- [ ] Passed values NOT declared as `@State` or `@StateObject`
- [ ] `@Binding` only where child modifies parent state
- [ ] Property wrapper selection follows ownership rules
- [ ] State ownership clear and intentional

### Modern APIs
- [ ] No deprecated modifiers (foregroundColor, cornerRadius, etc.)
- [ ] Using `NavigationStack` instead of `NavigationView`
- [ ] Using `Button` instead of `onTapGesture` when appropriate
- [ ] Using two-parameter or no-parameter `onChange()`

### View Composition
- [ ] Using modifiers over conditionals for state changes (maintains identity)
- [ ] Complex views extracted to separate subviews
- [ ] Views kept small and focused
- [ ] View `body` simple and pure (no side effects)

### Navigation & Sheets
- [ ] Using `navigationDestination(for:)` for type-safe navigation
- [ ] Using `.sheet(item:)` for model-based sheets
- [ ] Sheets own their dismiss actions

### Lists & Collections
- [ ] `ForEach` uses stable identity (never `.indices` for dynamic data)
- [ ] Constant number of views per `ForEach` element
- [ ] No inline filtering in `ForEach` (prefilter and cache)
- [ ] No `AnyView` in list rows

### Performance
- [ ] Passing only needed values to views (not large config objects)
- [ ] Eliminating unnecessary dependencies
- [ ] Checking value changes before state assignment in hot paths
- [ ] Using `LazyVStack`/`LazyHStack` for large lists
- [ ] No object creation in view `body`

### Async Work
- [ ] Using `.task` for automatic cancellation
- [ ] Using `.task(id:)` for value-dependent tasks
- [ ] Not mixing `.onAppear` with async work

See reference files for detailed explanations of each item.

## Quick Reference Guide

Load these references based on specific needs:

**State management questions** → `references/state.md`
- Property wrapper selection (@State, @Binding, @Observable)
- Data flow between views
- View model patterns

**Navigation questions** → `references/navigation.md`
- NavigationStack implementation
- Deep links and universal links
- Sheets, tabs, navigation state

**View structure questions** → `references/view-composition.md`
- Extracting views into components
- Parent/child data flow
- Container patterns

**List/collection questions** → `references/lists-collections.md`
- Stable identity with ForEach
- List vs LazyVStack
- Dynamic data patterns

**Scrolling questions** → `references/scrolling.md`
- Pagination and infinite scroll
- Scroll position and anchoring
- Loading content on scroll

**Async work questions** → `references/concurrency.md`
- When to use .task vs .onAppear vs .onChange
- Cancellation and lifecycle
- Updating UI from async contexts

**Performance questions** → `references/performance.md`
- View optimization strategies
- List scrolling optimization
- Memory management

**Testing questions** → `references/testing-di.md`
- Unit testing patterns
- Dependency injection
- Test doubles

**Refactoring tasks** → `references/refactor-playbooks.md`
- Step-by-step refactor guides
- View extraction playbooks
- Navigation migration
- State hoisting patterns

**Common patterns** → `references/patterns.md`
- Container views (loading/error states)
- Reusable ViewModifiers
- Environment-based DI
- PreferenceKeys

**Legacy migration** → `references/modern-swiftui-apis.md`
- Deprecated API replacements
- NavigationView → NavigationStack
- Property wrapper updates

**Code quality** → `references/code-review-refactoring.md`
- Code smells and anti-patterns
- Quality improvement checklist

## Core Constraints

- **Swift/SwiftUI focus only** - Exclude server-side Swift and UIKit unless bridging required
- **No Swift concurrency patterns** - Use `.task` for SwiftUI async work
- **No architecture mandates** - Don't require MVVM/MVC/VIPER or specific structures
- **No formatting/linting rules** - Focus on correctness and patterns
- **No tool-specific guidance** - No Xcode, Instruments, or IDE instructions
- **Citations allowed:** `developer.apple.com/documentation/swiftui/`, `developer.apple.com/documentation/swift/`

All workflows must follow these constraints.

## Examples

**Example 1: State management question**
User: "Should I use @State or @Binding for this counter?"
→ Check Quick Reference table, consult `references/state.md` if needed, answer directly

**Example 2: Review request**
User: "Review this view for issues"
→ Read `references/workflows-review.md`, apply Review Checklist, use Review Response Template

**Example 3: Refactor request**
User: "Extract this view into smaller components"
→ Read `references/workflows-refactor.md` and `references/refactor-playbooks.md`, follow extraction guide

**Example 4: Performance issue**
User: "My list scrolls slowly"
→ Read `references/performance.md` and `references/lists-collections.md`, diagnose and fix

**Example 5: Legacy migration**
User: "Update my NavigationView code"
→ Read `references/modern-swiftui-apis.md` and `references/refactor-playbooks.md`, migrate step-by-step

## Philosophy

This skill focuses on **facts and best practices** from Apple's documentation:
- Modern APIs over deprecated ones
- Clear state ownership patterns
- Performance-conscious view composition
- Testable code structure
- No architectural mandates (MVVM/VIPER not required)
- Apple Human Interface Guidelines adherence

## Reference Files

All references in `references/`:
- `workflows-review.md` - Review methodology and findings taxonomy
- `workflows-refactor.md` - Refactor methodology and invariants
- `refactor-playbooks.md` - Step-by-step refactor guides
- `state.md` - Property wrappers and ownership patterns
- `navigation.md` - Navigation implementation patterns
- `view-composition.md` - View structure and extraction
- `lists-collections.md` - Identity and ForEach patterns
- `scrolling.md` - Scroll handling and pagination
- `concurrency.md` - Async work with .task
- `performance.md` - Optimization strategies
- `testing-di.md` - Testing and dependency injection
- `patterns.md` - Common SwiftUI patterns
- `modern-swiftui-apis.md` - Legacy API migration
- `code-review-refactoring.md` - Code quality checks
