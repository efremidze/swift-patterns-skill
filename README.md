# Swift Patterns Skill

A comprehensive Swift/SwiftUI knowledge base for AI coding tools, following the [Agent Skills standard](https://agentskills.io/home).

Includes patterns for state management, navigation, view composition, performance, and modern API usage to improve AI-generated SwiftUI code.

## Want Dynamic Fetching?

If you need **runtime search, retrieval, and dynamic access** to Swift best practices, check out:

**[swift-patterns-mcp](https://github.com/efremidze/swift-patterns-mcp)**: An MCP server with intelligent search, persistent memory, and optional premium integrations.

**Key difference:**
- **swift-patterns-skill** (this repo) = Static guidance (portable, no runtime)
- **swift-patterns-mcp** = Dynamic tooling (search, retrieval, premium features)

## Quick Start

Install with a single command:

```bash
npx skills add https://github.com/efremidze/swift-patterns-skill --skill swift-patterns
```

Then use it in your AI assistant:
> Review my SwiftUI view for state management issues

[View on skills.sh →](https://skills.sh/efremidze/swift-patterns-skill/swift-patterns)

## What You Get

This skill teaches your AI assistant about:

- **Modern APIs** – iOS 17/18/26 replacements for deprecated APIs, migration guides
- **State Management** – Property wrapper selection (@State, @Binding, @Observable), ownership rules, data flow
- **Navigation** – NavigationStack, deep linking, sheets, type-safe routing
- **View Composition** – Extraction patterns, parent/child data flow, identity stability
- **Lists & Scrolling** – Stable identity, pagination, lazy containers
- **Performance** – View optimization, avoiding recomputation, memory management
- **Testing & DI** – Protocol-based dependency injection, test doubles
- **Refactoring** – Step-by-step playbooks for safe refactors

## How to Install

### Option A: Using `skills.sh` (recommended)

```bash
npx skills add https://github.com/efremidze/swift-patterns-skill --skill swift-patterns
```

### Option B: Claude Code Plugin

For personal usage in Claude Code:

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

### Option C: Manual Install

1. Clone this repository.
2. Install or symlink `swift-patterns/` to your tool's skills directory.
3. Use your AI tool and ask it to use `swift-patterns`.

## Skill Structure

```
swift-patterns/
  SKILL.md                        # Workflow routing, quick references, review checklist
  references/
    state.md                      # Property wrappers, ownership, @Observable
    navigation.md                 # NavigationStack, sheets, deep linking
    view-composition.md           # View extraction, data flow patterns
    lists-collections.md          # ForEach identity, List vs LazyVStack
    scrolling.md                  # Pagination, scroll position
    concurrency.md                # .task modifier, async lifecycle
    performance.md                # View optimization, lazy loading
    testing-di.md                 # Dependency injection, test doubles
    patterns.md                   # Container views, ViewModifiers, PreferenceKeys
    modern-swiftui-apis.md        # iOS 17/18/26 API replacements
    refactor-playbooks.md         # Step-by-step refactor guides
    workflows-review.md           # Review methodology
    workflows-refactor.md         # Refactor methodology, invariants
    code-review-refactoring.md    # Code smells, anti-patterns
```

## Other Skills

- [swift-architecture-skill](https://github.com/efremidze/swift-architecture-skill)

## Contributing

Contributions are welcome! This repository follows the [Agent Skills open format](https://agentskills.io/home), which has specific structural requirements.

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on improving the skill content and reference files.

## License

MIT License. See [LICENSE](LICENSE) for details.
