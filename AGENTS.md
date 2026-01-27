# Agent Guidelines for Swift Patterns

This document provides guidance for AI agents working with this skill to ensure consistency and avoid common pitfalls.

## Core Principles

### 1. Swift and SwiftUI Focus
**This is a Swift and SwiftUI skill.** Do not include:
- Server-side Swift patterns
- UIKit patterns (except when bridging is necessary)
- Deep Swift concurrency patterns (actors, Sendable, etc.) — use `.task` for SwiftUI async work when needed

### 2. No Code Formatting or Linting
**Do not include formatting/linting rules.** Avoid:
- Property ordering requirements (environment, state, body, etc.)
- Code organization mandates
- Whitespace or indentation rules
- Naming convention enforcement
- File structure requirements

**Exception**: Mention organization patterns as *optional suggestions* for readability, never as requirements.

### 3. No Architectural Opinions
**Stick to facts, not architectures.** Avoid:
- Enforcing MVVM, MVC, VIPER, or any specific architecture
- Mandating view model patterns
- Requiring specific folder structures
- Dictating dependency injection patterns
- Prescribing router/coordinator patterns

**Exception**: Suggest separating business logic for testability without enforcing how.

### 4. No Tool-Specific Instructions
**Agents cannot use external tools.** Do not include:
- Xcode Instruments profiling instructions
- Debugging tool usage
- IDE-specific features
- Command-line tool usage beyond basic git

**Exception**: Mention that users can profile with Instruments if performance issues arise, but don't provide detailed instructions.

## Content Guidelines

### Suggestions vs Requirements

**Use "suggest" or "consider" for optional optimizations:**
- ✅ "Consider downsampling images when using `UIImage(data:)`"
- ❌ "Always downsample images"

**Use "always" or "never" only for correctness issues:**
- ✅ "Never use `.indices` for dynamic ForEach content"
- ✅ "Always mark `@State` as `private`"

### Performance Optimizations

**Present performance optimizations as optional improvements:**
- Image downsampling: Suggest when `UIImage(data:)` is encountered
- POD view wrappers: Mention as advanced optimization technique
- Equatable conformance: Suggest for expensive views

**Do not automatically apply optimizations.** Let developers decide based on their performance needs.

### Modern API Usage

**Enforce modern API usage for correctness:**
- ✅ `foregroundStyle()` instead of `foregroundColor()`
- ✅ `NavigationStack` instead of `NavigationView`
- ✅ `@Observable` instead of `ObservableObject` for new code

These are about using current, non-deprecated APIs, not optimization.

### State Management

**Be clear about `@MainActor` requirements:**
- Mention that `@Observable` classes may need `@MainActor`
- Note that projects with default actor isolation don't need explicit `@MainActor`
- Don't mandate it as "always required"

## What to Include

### ✅ Include These Topics:
- Property wrapper selection (`@State`, `@Binding`, `@Observable`, etc.)
- Modern API replacements for deprecated APIs
- View composition and extraction patterns
- Performance patterns (stable identity, lazy loading, etc.)
- Common pitfalls and how to avoid them
- Sheet, navigation, and list patterns
- Liquid Glass API usage (iOS 26+)
- Accessibility best practices

### ❌ Exclude These Topics:
- Swift concurrency deep dives (actors, sendable, etc.)
- Code formatting and style rules
- Architectural patterns and mandates
- Tool usage instructions (Instruments, debuggers)
- File organization requirements
- Testing framework setup (XCTest configuration, CI pipelines)
- Build system configuration
- Project structure mandates

## Language and Tone

### Use Clear, Direct Language:
- "Use X instead of Y" (for deprecated APIs)
- "Consider X when Y" (for optimizations)
- "Avoid X because Y" (for anti-patterns)
- "X is preferred over Y" (for best practices)

### Avoid Prescriptive Language:
- ❌ "You must organize properties in this order"
- ❌ "Always use MVVM architecture"
- ❌ "Profile with Instruments following these steps"
- ❌ "Structure your project like this"

## Examples

### Good Example:
```markdown
## ForEach Identity

**Always provide stable identity for `ForEach`.** Never use `.indices` for dynamic content.

When you encounter `UIImage(data:)`, consider suggesting image downsampling as a performance optimization.
```

### Bad Example:
```markdown
## View Organization

**Always organize view properties in this order:**
1. Environment
2. State
3. Body
4. Helpers

**Use Instruments to profile:**
1. Open Instruments
2. Select Swift template
3. Record and analyze...
```

## Updating the Skill

When adding new content:
1. Ask: "Is this Swift-specific?"
2. Ask: "Is this a fact or an opinion?"
3. Ask: "Can agents actually use this?"
4. Ask: "Is this about correctness or style?"

If unsure, err on the side of excluding content. It's better to have a focused, factual skill than a comprehensive but opinionated one.

## Summary

**Focus**: Swift APIs, patterns, and correctness
**Avoid**: Formatting, architecture, tools, Swift language features
**Tone**: Factual, helpful, non-prescriptive
**Goal**: Make agents Swift experts without enforcing opinions
