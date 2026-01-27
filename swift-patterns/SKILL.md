---
name: swift-patterns
description: Swift and SwiftUI best practices for modern iOS development. Use when writing, reviewing, or refactoring code.
---

# swift-patterns

## Overview
Use this skill to build, review, or improve Swift and SwiftUI code with correct patterns for state management, navigation, testing, performance optimization, and maintainable code. This skill provides practical guidance for modern Swift development following Apple's recommended patterns and iOS best practices.

## Constraints
- Swift/SwiftUI focus only; exclude server-side Swift and UIKit patterns unless bridging is required.
- Avoid Swift concurrency patterns; use `.task` for SwiftUI async work when needed.
- No architecture mandates (do not require MVVM/MVC/VIPER, coordinators, or specific folder structures).
- No formatting or linting rules.
- No tool-specific steps (Xcode, Instruments, IDE guidance, or CLI walkthroughs).
- Citations allowed: `developer.apple.com/documentation/swiftui/`, `developer.apple.com/documentation/swift/`, `developer.apple.com/documentation/swift/concurrency`.

All workflows below must follow the Constraints section to prevent drift.

## Refactor Response Template
1) Intent + scope (what is being changed and why)
2) Changes (bulleted list with file paths)
3) Behavior preservation checks (what should remain unchanged)
4) Constraints check (confirm alignment)
5) Next steps (tests or verification, if applicable)

## Review Response Template
1) Scope (what was reviewed)
2) Findings (grouped by severity with actionable statements)
3) Evidence (file paths or code locations for each finding)
4) Risks and tradeoffs (what could break or needs attention)
5) Constraints check (confirm alignment)
6) Next steps (what to fix first or verify)

## Workflow Routing

Route requests based on intent cues:

| Signal | Route |
| --- | --- |
| "review", "find issues", "audit", "assess" | [workflows-review.md](references/workflows-review.md) |
| "refactor", "change", "implement", "improve" | [workflows-refactor.md](references/workflows-refactor.md) |
| "no tests", "legacy", "migration" | Refactor with extra caution |

If unclear, ask: "Do you want findings only (review), or should I change the code (refactor)?"

## When to Use Which Reference

### Choose `references/state.md` when:
- Choosing property wrappers for state
- Designing data flow in views
- Structuring app architecture
- Managing view models and dependencies
- Building reusable view components

### Choose `references/navigation.md` when:
- Implementing navigation flows
- Handling deep links or universal links
- Managing navigation state
- Supporting state restoration
- Working with sheets, tabs, or navigation stacks

### Choose `references/testing-di.md` when:
- Writing unit tests for Swift code
- Setting up dependency injection
- Creating test doubles (mocks/fakes/spies)
- Structuring testable architecture

### Choose `references/performance.md` when:
- Optimizing SwiftUI view performance
- Improving list scrolling performance
- Managing memory efficiently
- Implementing caching strategies
- Profiling and measuring performance

### Choose `references/code-review-refactoring.md` when:
- Reviewing code for quality issues
- Identifying code smells
- Planning refactoring work
- Applying design patterns
- Improving code maintainability

### Choose `references/view-composition.md` when:
- Extracting views into smaller components
- Deciding where state should live
- Fixing parent/child data flow issues
- Applying layout patterns

### Choose `references/lists-collections.md` when:
- Building lists with stable identity
- Using ForEach with dynamic data
- Choosing between List and LazyVStack
- Fixing selection or state loss in lists

### Choose `references/scrolling.md` when:
- Implementing pagination or infinite scroll
- Handling scroll position and anchoring
- Loading more content on scroll
- Fixing scroll-related performance issues

### Choose `references/concurrency.md` when:
- Running async work in SwiftUI views
- Choosing between .task, .onAppear, and .onChange
- Handling cancellation and lifecycle
- Updating UI from async contexts

### Choose `references/modern-swiftui-apis.md` when:
- Replacing deprecated SwiftUI APIs
- Migrating from NavigationView to NavigationStack
- Updating to modern property wrappers
- Finding current replacements for legacy patterns

### Choose `references/refactor-playbooks.md` when:
- Extracting views while preserving behavior
- Migrating navigation to NavigationStack
- Hoisting state without breaking bindings
- Following step-by-step refactor guides

### Choose `references/patterns.md` when:
- Building container views for loading/error states
- Creating reusable ViewModifiers for styling
- Setting up environment-based dependency injection
- Using PreferenceKeys for child-to-parent communication

## Reference Files

- **workflows-review.md** - Review checklist, findings taxonomy, risk cues
- **workflows-refactor.md** - Refactor checklist, invariants, risk cues
- **refactor-playbooks.md** - Step-by-step playbooks for view extraction, navigation migration, state hoisting
- **state.md** - Property wrapper selection, ownership rules, tradeoffs
- **navigation.md** - NavigationStack, sheets, deep linking, state restoration
- **view-composition.md** - View extraction, parent/child data flow, layout
- **lists-collections.md** - Stable identity, ForEach, List vs LazyVStack
- **scrolling.md** - Pagination triggers, scroll position, lazy loading
- **concurrency.md** - .task, .onChange, cancellation, @MainActor
- **performance.md** - View optimization, identity stability, lazy containers
- **testing-di.md** - Protocol-based DI, test doubles, testable structure
- **patterns.md** - Container views, ViewModifiers, PreferenceKeys, Environment DI
- **modern-swiftui-apis.md** - Legacy API replacement catalog
- **code-review-refactoring.md** - Code smells, anti-patterns, quality checks

