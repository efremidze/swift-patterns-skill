# Swift Patterns Skill

A comprehensive Swift/SwiftUI knowledge base for AI coding tools, following the [Agent Skills standard](https://agentskills.io/home).

Includes patterns for state management, navigation, view composition, performance, and modern API usage to improve AI-generated SwiftUI code.

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

## Installation Options

### Using skills.sh (Recommended)

```bash
npx skills add https://github.com/efremidze/swift-patterns-skill --skill swift-patterns
```

### Claude Code Plugin

Install for personal use:

```bash
/plugin marketplace add efremidze/swift-patterns-skill
/plugin install efremidze@swift-patterns
```

Or configure for your team in `.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "efremidze@swift-patterns": true
  },
  "extraKnownMarketplaces": {
    "swift-patterns": {
      "source": {
        "source": "github",
        "repo": "efremidze/swift-patterns-skill"
      }
    }
  }
}
```

### Manual Installation

1. Clone this repository
2. Copy or symlink the `swift-patterns/` folder to your AI tool's skills directory
3. Refer to your tool's documentation:
   - [Cursor](https://cursor.com/docs/context/skills#enabling-skills)
   - [Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview#using-skills)
   - [GitHub Copilot](https://docs.github.com/copilot)

## What Makes This Different

**Practical** – Includes actionable checklists, decision trees, and code examples rather than abstract theory.

**Modern** – Comprehensive coverage of iOS 17, 18, and 26 APIs with migration guides.

**Flexible** – Provides guidance without enforcing specific architectures (no MVVM/VIPER mandates).

**Focused** – SwiftUI patterns only. No server-side Swift, UIKit (unless bridging), or tool-specific guidance.

## Structure

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

## Contributing

Contributions welcome! This repository follows the [Agent Skills open format](https://agentskills.io/home).

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on improving the skill content and reference files.

## License

MIT License. See [LICENSE](LICENSE) for details.
