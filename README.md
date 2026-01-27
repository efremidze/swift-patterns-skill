# Swift Patterns Skill

A comprehensive Swift and SwiftUI knowledge base for AI coding tools, following the [Agent Skills standard](https://agentskills.io/home).

Includes patterns for concurrency, state management, navigation, testing, and performance to improve AI-generated code quality.

## Quick Start

Install with a single command:

```bash
npx skills add https://github.com/efremidze/swift-patterns-skill --skill swift-patterns
```

Then use it in your AI assistant:
> Use the Swift expert skill to review my async/await implementation

[View on skills.sh →](https://skills.sh/efremidze/swift-patterns-skill/swift-patterns)

## What You Get

This skill teaches your AI assistant about:

- **SwiftUI Architecture** – state management, property wrappers, data flow, and view composition  
- **Swift Concurrency** – async/await patterns, actors, structured concurrency, and cancellation
- **Navigation** – NavigationStack, deep linking, and coordinators
- **Testing & DI** – unit testing, dependency injection, and test doubles
- **Performance** – view optimization, memory management, and profiling
- **Code Quality** – identifying code smells, refactoring patterns, and SOLID principles

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

**Comprehensive** – Covers all major aspects of Swift and SwiftUI in one unified skill, not scattered across multiple repositories.

**Practical** – Includes real-world examples, tradeoffs, and decision trees rather than abstract theory.

**Modern** – Focuses on current Swift concurrency, SwiftUI patterns, and iOS best practices.

**Flexible** – Provides guidance without enforcing specific architectures or project structures.

## Structure

```
swift-patterns/
  SKILL.md                        # Main workflow and decision trees
  references/
    concurrency.md                # Async/await, tasks, actors
    state.md                      # State management and data flow
    navigation.md                 # Navigation patterns
    testing-di.md                 # Testing and dependency injection
    performance.md                # Performance optimization
    code-review-refactoring.md    # Code quality and refactoring
```

## Contributing

Contributions welcome! This repository follows the [Agent Skills open format](https://agentskills.io/home).

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on improving the skill content and reference files.

## License

MIT License. See [LICENSE](LICENSE) for details.
