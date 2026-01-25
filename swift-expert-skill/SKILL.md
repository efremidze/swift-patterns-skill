---
name: swift-expert-skill
description: Expert guidance for Swift, SwiftUI, and iOS engineering. Write, review, or improve Swift code following best practices for concurrency, state management, navigation, testing, performance, and code quality. Use when building new features, refactoring existing code, reviewing code quality, or adopting modern Swift patterns.
---

# Swift Expert Skill

## Overview
Use this skill to build, review, or improve Swift and SwiftUI code with correct patterns for concurrency, state management, navigation, testing, performance optimization, and maintainable architecture. This skill provides practical guidance for modern Swift development following Apple's recommended patterns and iOS best practices.

## Workflow Decision Tree

### 1) Review existing Swift/SwiftUI code
- Check Swift concurrency usage (async/await, actors, structured concurrency) (see `references/swift-concurrency.md`)
- Verify SwiftUI property wrapper usage and state management (see `references/swiftui-architecture.md`)
- Review navigation patterns and deep linking (see `references/navigation.md`)
- Check testing patterns and dependency injection (see `references/testing-di.md`)
- Verify performance optimizations are applied (see `references/performance.md`)
- Review code for smells and refactoring opportunities (see `references/code-review-refactoring.md`)

### 2) Improve existing Swift/SwiftUI code
- Replace completion handlers with async/await using continuations (see `references/swift-concurrency.md`)
- Replace incorrect property wrappers and improve state management (see `references/swiftui-architecture.md`)
- Modernize navigation to use NavigationStack and type-safe routing (see `references/navigation.md`)
- Add protocol-based dependency injection for testability (see `references/testing-di.md`)
- Apply performance optimizations for lists, views, and memory (see `references/performance.md`)
- Refactor code smells using established patterns (see `references/code-review-refactoring.md`)

### 3) Implement new Swift/SwiftUI feature
- Design async operations with structured concurrency (see `references/swift-concurrency.md`)
- Choose appropriate property wrappers for state ownership (see `references/swiftui-architecture.md`)
- Implement navigation with centralized routing (see `references/navigation.md`)
- Structure code with testable dependency injection (see `references/testing-di.md`)
- Build performant views with proper optimizations (see `references/performance.md`)
- Follow SOLID principles and maintainable patterns (see `references/code-review-refactoring.md`)

## Core Guidelines

### Swift Concurrency
- **Always prefer async/await over completion handlers** for new code
- **Use structured concurrency** (async let, TaskGroup) instead of unstructured tasks
- **Protect mutable state with actors** instead of locks or DispatchQueue
- **Mark UI-related code with @MainActor** for thread safety
- **Handle cancellation properly** in long-running operations
- Never mix actors with DispatchQueue or blocking operations on MainActor

### SwiftUI State Management
- **Choose the right property wrapper** based on ownership (@State, @Binding, @StateObject, @ObservedObject)
- **Prefer @Observable over ObservableObject** for iOS 17+ (less boilerplate, better performance)
- **Follow unidirectional data flow** (data down, events up)
- **Keep views simple and declarative** (no logic in body, no side effects)
- **Use .task for async work** and .onChange for reactions
- Inject dependencies through initializers for testability

### Navigation
- **Use NavigationStack** instead of deprecated NavigationView
- **Centralize navigation logic** in coordinators or route enums
- **Implement type-safe navigation** with navigationDestination(for:)
- **Handle deep links** with proper URL parsing and validation
- **Support state restoration** for navigation paths
- Keep navigation state in a single source of truth

### Testing & Dependency Injection
- **Write tests first** for critical business logic
- **Use protocol-based DI** for service abstractions
- **Create proper test doubles** (mocks, fakes, spies) - not stubs for everything
- **Test async code** with proper cancellation and error handling
- **Keep business logic separate** from views and UI
- Structure code for testability from the start

### Performance
- **Optimize SwiftUI views** with proper state management and view identity
- **Use lazy loading** for large lists and data sets
- **Implement pagination** for network data
- **Apply caching strategies** for expensive operations
- **Profile before optimizing** with Instruments
- Avoid premature optimization - measure first

### Code Quality
- **Identify and refactor code smells** (god objects, long methods, duplicated code)
- **Follow SOLID principles** (single responsibility, dependency inversion)
- **Extract methods** to improve readability and testability
- **Use composition over inheritance** when possible
- **Keep functions small and focused** (single responsibility)
- Write self-documenting code with clear naming

## When to Use Which Reference

### Choose `references/swift-concurrency.md` when:
- Implementing async operations or network calls
- Managing concurrent tasks or parallel operations
- Working with actors or thread-safe state
- Converting legacy closure-based APIs
- Handling task cancellation

### Choose `references/swiftui-architecture.md` when:
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
- Testing async operations
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

## Quick Decision Guide

**Question: "How do I handle this async operation?"**
→ See `references/swift-concurrency.md`

**Question: "Which property wrapper should I use?"**
→ See `references/swiftui-architecture.md`

**Question: "How do I implement navigation to this screen?"**
→ See `references/navigation.md`

**Question: "How do I make this testable?"**
→ See `references/testing-di.md`

**Question: "Why is this view/list so slow?"**
→ See `references/performance.md`

**Question: "Is this code well-structured?"**
→ See `references/code-review-refactoring.md`

## Reference Files

All detailed patterns, examples, and best practices are organized in the `references/` directory:

- **swift-concurrency.md** - Async/await, Tasks, Actors, structured concurrency patterns
- **swiftui-architecture.md** - State management, property wrappers, data flow, architecture
- **navigation.md** - NavigationStack, deep linking, coordinators, state restoration
- **testing-di.md** - Unit testing, dependency injection, test doubles, async testing
- **performance.md** - SwiftUI optimization, memory management, profiling, caching
- **code-review-refactoring.md** - Code smells, refactoring patterns, SOLID principles

## Usage Tips

- Start with the Workflow Decision Tree to determine your task type (review/improve/implement)
- Use the Quick Decision Guide to find the right reference quickly
- Reference documents contain detailed guidelines, tradeoffs, code examples, and anti-patterns
- Each reference is self-contained but cross-references related topics
- Apply multiple references together for comprehensive solutions

## Example Workflows

### Workflow 1: Building a new feature with async data
1. Review `references/swift-concurrency.md` for async patterns
2. Review `references/swiftui-architecture.md` for state management
3. Review `references/testing-di.md` for testable structure
4. Implement feature following all three references

### Workflow 2: Optimizing a slow list
1. Review `references/performance.md` for list optimization
2. Review `references/swiftui-architecture.md` for state management issues
3. Profile with Instruments to measure improvements
4. Apply optimizations iteratively

### Workflow 3: Refactoring legacy code
1. Review `references/code-review-refactoring.md` to identify smells
2. Review `references/swift-concurrency.md` to modernize async code
3. Review `references/testing-di.md` to add testability
4. Refactor incrementally with tests

This skill combines expertise across all areas of modern Swift and SwiftUI development, providing a comprehensive guide for building high-quality iOS applications.
