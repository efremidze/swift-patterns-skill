# Swift Skills

Expert guidance for Swift, SwiftUI, and iOS engineering following the [Agent Skills open format](https://agentskills.io/home). This skill provides comprehensive best practices for modern Swift development with AI-assisted coding.

## Who this is for
- Swift developers using AI coding assistants (Cursor, Claude Code, Copilot, etc.)
- Teams adopting modern Swift concurrency and SwiftUI patterns
- Developers reviewing or refactoring Swift/SwiftUI codebases
- Anyone wanting AI guidance for Swift best practices

## How to Use This Skill

### Option A: Using skills.sh (recommended)
Install this skill with a single command:

```bash
npx skills add https://github.com/efremidze/swift-skills --skill swift-expert-skill
```

For more information, [visit the skills.sh platform page](https://skills.sh/efremidze/swift-skills/swift-expert-skill).

Then use the skill in your AI agent, for example:
> Use the swift expert skill and review my async/await implementation

### Option B: Claude Code Plugin

#### Personal Usage
To install this Skill for your personal use in Claude Code:

1. Add the marketplace:

```bash
/plugin marketplace add efremidze/swift-skills
```

2. Install the Skill:

```bash
/plugin install swift-expert@swift-expert-skill
```

#### Project Configuration
To automatically provide this Skill to everyone working in a repository, configure the repository's `.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "swift-expert@swift-expert-skill": true
  },
  "extraKnownMarketplaces": {
    "swift-expert-skill": {
      "source": {
        "source": "github",
        "repo": "efremidze/swift-skills"
      }
    }
  }
}
```

When team members open the project, Claude Code will prompt them to install the Skill.

### Option C: Manual install
1) **Clone** this repository.
2) **Install or symlink** the `swift-expert-skill/` folder following your tool's official skills installation docs (see links below).
3) **Use your AI tool** as usual and ask it to use the "swift expert" skill for Swift tasks.

#### Where to Save Skills
Follow your tool's official documentation:
- **Cursor:** [Enabling Skills](https://cursor.com/docs/context/skills#enabling-skills)
- **Claude:** [Using Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview#using-skills)
- **GitHub Copilot:** Skills are loaded contextually through the repository

**How to verify**:

Your agent should reference the workflow/checklists in `swift-expert-skill/SKILL.md` and jump into the relevant reference file for your task.

## What This Skill Offers

This skill gives your AI coding tool practical Swift guidance across all major areas:

### Guide Your Swift Decisions
- Choose the right concurrency primitives (async/await, Task, Actor)
- Select appropriate property wrappers (@State, @Binding, @Observable)
- Design navigation flows with NavigationStack and type-safe routing
- Structure testable code with proper dependency injection
- Apply performance optimizations effectively
- Identify and fix code quality issues

### Write Better Swift Code
- Implement structured concurrency patterns safely
- Build performant SwiftUI views with proper state management
- Handle cancellation and errors in async operations
- Create maintainable, testable architectures
- Follow modern iOS patterns and best practices

### Improve Code Quality
- Identify and fix code smells
- Apply refactoring patterns effectively
- Optimize SwiftUI performance
- Follow SOLID principles and maintainable patterns

## What Makes This Skill Different

**Comprehensive Coverage**: Covers all major areas of Swift and SwiftUI development in one unified skill.

**Practical & Actionable**: Provides concrete examples, tradeoffs, and decision trees for real-world scenarios.

**Modern-first**: Focuses on modern Swift concurrency, SwiftUI patterns, and current iOS best practices.

**Non-Opinionated**: Provides guidance without forcing specific architectures or project structures.

## Skill Structure
<!-- BEGIN REFERENCE STRUCTURE -->
```text
swift-expert-skill/
  SKILL.md                              # Main workflow and decision trees
  references/
    swift-concurrency.md                # Async/await, Tasks, Actors, structured concurrency
    swiftui-architecture.md             # State management, property wrappers, data flow
    navigation.md                       # NavigationStack, deep linking, coordinators
    testing-di.md                       # Unit testing, dependency injection, test doubles
    performance.md                      # SwiftUI performance, memory, profiling
    code-review-refactoring.md          # Code smells, refactoring patterns, SOLID
```
<!-- END REFERENCE STRUCTURE -->

## Reference Topics

The skill includes detailed reference documentation for:

- **Swift Concurrency** - Modern async/await patterns, actors, structured concurrency, cancellation handling
- **SwiftUI Architecture** - State management, property wrappers, data flow, view composition
- **Navigation** - NavigationStack, deep linking, state restoration, coordinators
- **Testing & DI** - Unit testing, dependency injection, test doubles, async testing
- **Performance** - View optimization, memory management, lazy loading, profiling
- **Code Quality** - Code smells, refactoring patterns, SOLID principles, maintainability

## 🤝 Contributing

This skill is designed for AI agents and human developers. Contributions welcome! This repository follows the [Agent Skills open format](https://agentskills.io/home), which has specific structural requirements.

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for:
- How to contribute improvements to `SKILL.md` and the reference files
- Format requirements and quality standards
- Pull request process

## License

This repository is open-source and available under the MIT License. See [LICENSE](LICENSE) for details.