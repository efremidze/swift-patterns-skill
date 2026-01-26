---
name: swift-agent-skill
description: Expert guidance for modern Swift and SwiftUI development. Use when Claude needs to: (1) review SwiftUI code for issues, (2) refactor Swift/SwiftUI views safely, (3) fix state management or property wrapper problems, (4) migrate navigation to NavigationStack, (5) improve list performance or fix identity issues, (6) audit code for SwiftUI anti-patterns, or (7) extract views while preserving behavior.
---

# Swift Expert Skill

## Overview
Use this skill to build, review, or improve Swift and SwiftUI code with correct patterns for state management, navigation, testing, performance optimization, and maintainable code. This skill provides practical guidance for modern Swift development following Apple's recommended patterns and iOS best practices.

## Constraints
- Swift/SwiftUI focus only; exclude server-side Swift and UIKit patterns unless bridging is required.
- Avoid Swift concurrency patterns; use `.task` for SwiftUI async work when needed.
- No architecture mandates (do not require MVVM/MVC/VIPER, coordinators, or specific folder structures).
- No formatting or linting rules.
- No tool-specific steps (Xcode, Instruments, IDE guidance, or CLI walkthroughs).
- Citations must only reference URLs listed in `references/sources.md`.

All workflows below must follow the Constraints section to prevent drift.

## Refactor Response Template
1) Intent + scope (what is being changed and why)
2) Changes (bulleted list with file paths)
3) Behavior preservation checks (what should remain unchanged)
4) Constraints + citation allowlist check (confirm alignment with Constraints and `references/sources.md`)
5) Next steps (tests or verification, if applicable)

## Review Response Template
1) Scope (what was reviewed)
2) Findings (grouped by severity with actionable statements)
3) Evidence (file paths or code locations for each finding)
4) Risks and tradeoffs (what could break or needs attention)
5) Constraints + citation allowlist check (confirm alignment with Constraints and `references/sources.md`)
6) Next steps (what to fix first or verify)

## Workflow Routing

Use [references/decisions.md](references/decisions.md) as the authoritative routing guide for selecting review vs refactor workflows based on explicit intent cues. This routing gate avoids duplicating rules and keeps workflow selection consistent.

After routing, use the workflow checklists:
- [references/workflows-review.md](references/workflows-review.md)
- [references/workflows-refactor.md](references/workflows-refactor.md)

All workflows must follow the Constraints section above.

## Core Guidelines

### SwiftUI State Management
- **Choose the right property wrapper** based on ownership (@State, @Binding, @Environment, @Observable)
- **Prefer @Observable over ObservableObject** for iOS 17+; use ObservableObject for earlier OS support
- **Follow unidirectional data flow** (data down, events up)
- **Keep views simple and declarative** (no logic in body, no side effects)
- **Use .task for async work** and .onChange for reactions
- Inject dependencies through initializers for testability

### Navigation
- **Use NavigationStack** instead of deprecated NavigationView
- **Consider centralized routing** with route enums when it improves clarity
- **Implement type-safe navigation** with navigationDestination(for:)
- **Handle deep links** with proper URL parsing and validation
- **Support state restoration** for navigation paths
- Keep navigation state in a single source of truth

### Testing & Dependency Injection
- **Write tests first** for critical business logic
- **Use protocol-based DI** for service abstractions
- **Create proper test doubles** (mocks, fakes, spies) - not stubs for everything
- **Keep business logic separate** from views and UI
- Structure code for testability from the start

### Performance
- **Optimize SwiftUI views** with proper state management and view identity
- **Use lazy loading** for large lists and data sets
- **Implement pagination** for network data
- **Apply caching strategies** for expensive operations
- Avoid premature optimization - measure first

### Code Quality
- **Identify and refactor code smells** (god objects, long methods, duplicated code)
- **Extract methods** to improve readability and testability
- **Use composition over inheritance** when possible
- **Keep functions small and focused** (single responsibility)
- Write self-documenting code with clear naming

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

## Quick Decision Guide

**Question: "Which property wrapper should I use?"**
→ See `references/state.md`

**Question: "How do I implement navigation to this screen?"**
→ See `references/navigation.md`

**Question: "How do I build lists with stable identity?"**
→ See `references/lists-collections.md`

**Question: "How do I paginate or handle scrolling safely?"**
→ See `references/scrolling.md`

**Question: "How do I split this view into smaller pieces?"**
→ See `references/view-composition.md`

**Question: "How do I refactor safely (view extraction, navigation migration, state hoisting)?"**
→ See `references/refactor-playbooks.md`

**Question: "What replaces this legacy SwiftUI API?"**
→ See `references/modern-swiftui-apis.md`

**Question: "How do I make this testable?"**
→ See `references/testing-di.md`

**Question: "Why is this view/list so slow?"**
→ See `references/performance.md`

**Question: "How do I run async work in SwiftUI safely?"**
→ See `references/concurrency.md`

**Question: "Is this code well-structured?"**
→ See `references/code-review-refactoring.md`

**Question: "How do I build a reusable loading/error container?"**
→ See `references/patterns.md`

## Reference Files

All detailed patterns, examples, and best practices are organized in the `references/` directory:

- **decisions.md** - Review vs refactor routing gates and intent cues
- **workflows-review.md** - Review checklist, findings taxonomy, and risk cues
- **workflows-refactor.md** - Behavior-preserving refactor checklist and risk cues
- **refactor-playbooks.md** - Goal-based refactor playbooks for common migrations
- **state.md** - State management, property wrappers, ownership guidance
- **view-composition.md** - View extraction, data flow, and invariants
- **navigation.md** - NavigationStack, deep linking, routing, state restoration
- **lists-collections.md** - Stable identity, lazy containers, list composition
- **scrolling.md** - ScrollView, pagination triggers, safe loading
- **modern-swiftui-apis.md** - Replacement catalog for legacy SwiftUI APIs
- **concurrency.md** - SwiftUI lifecycle-aware async work and @MainActor updates
- **testing-di.md** - Unit testing, dependency injection, test doubles
- **performance.md** - SwiftUI optimization, memory management, profiling, caching
- **code-review-refactoring.md** - Code smells and refactoring patterns
- **patterns.md** - Reusable SwiftUI patterns (containers, ViewModifiers, PreferenceKeys, Environment DI)

## Usage Tips

- Start with Workflow Routing to select review vs refactor intent
- Use the Quick Decision Guide to find the right reference quickly
- Reference documents contain detailed guidelines, tradeoffs, code examples, and anti-patterns
- Each reference is self-contained but cross-references related topics
- Apply multiple references together for comprehensive solutions

## Example Workflows

### Workflow 1: Building a new feature with data loading
1. Review `references/state.md` for state management
2. Review `references/testing-di.md` for testable structure
3. Review `references/performance.md` for data-loading optimizations
4. Implement feature following all three references

### Workflow 2: Optimizing a slow list
1. Review `references/performance.md` for list optimization
2. Review `references/state.md` for state management issues
3. Measure improvements with your preferred tooling if needed
4. Apply optimizations iteratively

### Workflow 3: Refactoring legacy code
1. Review `references/code-review-refactoring.md` to identify smells
2. Review `references/testing-di.md` to add testability
3. Refactor incrementally with tests

This skill combines expertise across all areas of modern Swift and SwiftUI development, providing a comprehensive guide for building high-quality iOS applications.
