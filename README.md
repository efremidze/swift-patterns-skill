# Swift Patterns Skill

[![Release](https://img.shields.io/github/v/release/efremidze/swift-patterns-skill?sort=semver)](https://github.com/efremidze/swift-patterns-skill/releases)
[![Validate Skill](https://github.com/efremidze/swift-patterns-skill/actions/workflows/validate-skill.yml/badge.svg)](https://github.com/efremidze/swift-patterns-skill/actions/workflows/validate-skill.yml)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-Compatible-purple.svg)](https://agentskills.io/home)

A comprehensive Swift/SwiftUI knowledge base for AI coding tools, following the [Agent Skills standard](https://agentskills.io/home).

Provides expert guidance on state management, navigation, view composition, performance optimization, and modern SwiftUI API usage to help AI assistants generate better SwiftUI code.

## Quick Start

Install with a single command:

```bash
npx skills add https://github.com/efremidze/swift-patterns-skill --skill swift-patterns
```

Then use it in your AI assistant:
> Review my SwiftUI view for state management issues

[View on skills.sh →](https://skills.sh/efremidze/swift-patterns-skill/swift-patterns)

## What This Skill Provides

Comprehensive SwiftUI expertise across:

### Core Topics
- **State Management** – Property wrapper selection (`@State`, `@Binding`, `@Observable`), ownership rules, data flow patterns
- **Modern APIs** – iOS 17/18/26 replacements for deprecated APIs, complete migration guides
- **View Composition** – Extraction patterns, parent/child data flow, view identity and performance
- **Navigation** – `NavigationStack`, sheets, deep linking, type-safe routing patterns

### Advanced Areas
- **Lists & Collections** – Stable identity with `ForEach`, pagination, lazy containers
- **Performance Optimization** – View optimization strategies, avoiding recomputation, memory management
- **Testing & Dependency Injection** – Protocol-based patterns, test doubles, testable architecture
- **Code Quality** – Refactoring playbooks, code smell detection, anti-pattern identification

All guidance is based on Apple's official documentation and focuses on **facts over opinions** – no architectural mandates.

## Installation

### Recommended: Using skills.sh CLI

The easiest way to install:

```bash
npx skills add https://github.com/efremidze/swift-patterns-skill --skill swift-patterns
```

This installs the skill and makes it available to your AI assistant.

### Alternative: Claude Code Plugin

For Claude Code users, add via the marketplace:

1. Add the marketplace:
   ```bash
   /plugin marketplace add efremidze/swift-patterns-skill
   ```

2. Install the skill:
   ```bash
   /plugin install swift-patterns@swift-patterns-skill
   ```

Or configure for your team in `.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "swift-patterns@swift-patterns-skill": true
  },
  "extraKnownMarketplaces": {
    "swift-patterns-skill": {
      "source": {
        "source": "github",
        "repo": "efremidze/swift-patterns-skill"
      }
    }
  }
}
```

### Manual Installation

If you prefer manual setup:

1. Clone this repository
2. Install or symlink `swift-patterns/` to your tool's skills directory
3. Configure your AI tool to use `swift-patterns`

## Skill Structure

The skill follows a progressive disclosure model—core workflows in `SKILL.md`, detailed guidance in `references/`:

```
swift-patterns/
  SKILL.md                          # Entry point: workflow routing, quick refs, review checklist
  references/
    state.md                        # Property wrappers, ownership, @Observable patterns
    navigation.md                   # NavigationStack, sheets, deep linking
    view-composition.md             # View extraction, data flow patterns
    lists-collections.md            # ForEach identity, List vs LazyVStack
    scrolling.md                    # Pagination, scroll position management
    concurrency.md                  # .task modifier, async lifecycle
    performance.md                  # View optimization, lazy loading strategies
    testing-di.md                   # Dependency injection, test doubles
    patterns.md                     # Container views, ViewModifiers, PreferenceKeys
    modern-swiftui-apis.md          # iOS 17/18/26 API replacements and migration
    refactor-playbooks.md           # Step-by-step refactoring guides
    workflows-review.md             # Review methodology and standards
    workflows-refactor.md           # Refactoring methodology, invariants
    code-review-refactoring.md      # Code smells, anti-patterns, quality checks
```

## Related Projects

### Other Skills
- **[swift-architecture-skill](https://github.com/efremidze/swift-architecture-skill)** – Architectural patterns and project structure guidance (complements this skill's focus on SwiftUI patterns)

### Dynamic Runtime Tools
- **[swift-patterns-mcp](https://github.com/efremidze/swift-patterns-mcp)** – MCP server with intelligent search, retrieval, and persistent memory

**Key difference:**
- **swift-patterns-skill** (this repo) = Static guidance, portable, no runtime dependencies
- **swift-patterns-mcp** = Dynamic tooling with search, retrieval, and premium integrations

## Contributing

Contributions are welcome! This repository follows the [Agent Skills open format](https://agentskills.io/home).

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on improving the skill content and reference files.

## License

MIT License. See [LICENSE](LICENSE) for details.
