# Swift Skills

A comprehensive collection of agent skills for Swift, SwiftUI, and iOS engineering best practices following the [Agent Skills open format](https://agentskills.io/home). Each skill provides clear rules, tradeoffs, and practical Swift examples for AI-assisted development.

## Who this is for
- Swift developers using AI coding assistants (Cursor, Claude Code, Copilot, etc.)
- Teams adopting modern Swift concurrency and SwiftUI patterns
- Developers reviewing or refactoring Swift/SwiftUI codebases
- Anyone wanting AI guidance for Swift best practices

## How to Use This Skill

### Option A: Using skills.sh (recommended)
Install skills with a single command:

```bash
npx skills add https://github.com/efremidze/swift-skills --skill swift-concurrency
npx skills add https://github.com/efremidze/swift-skills --skill swiftui-architecture
npx skills add https://github.com/efremidze/swift-skills --skill navigation
npx skills add https://github.com/efremidze/swift-skills --skill testing-di
npx skills add https://github.com/efremidze/swift-skills --skill code-review-refactoring
npx skills add https://github.com/efremidze/swift-skills --skill performance
```

Then use the skills in your AI agent, for example:
> Use the swift-concurrency skill and review my async/await implementation

### Option B: Claude Code Plugin

#### Personal Usage
To install these Skills for your personal use in Claude Code:

1. Add the marketplace:

```bash
/plugin marketplace add efremidze/swift-skills
```

2. Install the Skills:

```bash
/plugin install swift-concurrency@swift-concurrency
/plugin install swiftui-architecture@swiftui-architecture
/plugin install navigation@navigation
/plugin install testing-di@testing-di
/plugin install code-review-refactoring@code-review-refactoring
/plugin install performance@performance
```

#### Project Configuration
To automatically provide these Skills to everyone working in a repository, configure the repository's `.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "swift-concurrency@swift-concurrency": true,
    "swiftui-architecture@swiftui-architecture": true,
    "navigation@navigation": true,
    "testing-di@testing-di": true,
    "code-review-refactoring@code-review-refactoring": true,
    "performance@performance": true
  },
  "extraKnownMarketplaces": {
    "swift-skills": {
      "source": {
        "source": "github",
        "repo": "efremidze/swift-skills"
      }
    }
  }
}
```

When team members open the project, Claude Code will prompt them to install the Skills.

### Option C: Manual install
1) **Clone** this repository.
2) **Install or symlink** the skill folders following your tool's official skills installation docs (see links below).
3) **Use your AI tool** as usual and ask it to use the specific skill for Swift tasks.

#### Where to Save Skills
Follow your tool's official documentation:
- **Cursor:** [Enabling Skills](https://cursor.com/docs/context/skills#enabling-skills)
- **Claude:** [Using Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview#using-skills)
- **GitHub Copilot:** Skills are loaded contextually through the repository

## 📚 Available Skills

### Core Swift
- **[swift-concurrency](swift-concurrency/SKILL.md)** - Best practices for async/await, Tasks, Actors, and structured concurrency

### SwiftUI & Architecture
- **[swiftui-architecture](swiftui-architecture/SKILL.md)** - Patterns for managing state, data flow, and app architecture
- **[navigation](navigation/SKILL.md)** - Modern navigation patterns with NavigationStack, deep linking, and state management

### Testing & Quality
- **[testing-di](testing-di/SKILL.md)** - Unit testing patterns, DI approaches, and testing async code
- **[code-review-refactoring](code-review-refactoring/SKILL.md)** - Code smells, refactoring patterns, and review guidelines

### Performance
- **[performance](performance/SKILL.md)** - SwiftUI performance, memory management, and profiling

## Skill Structure
<!-- BEGIN REFERENCE STRUCTURE -->
```text
swift-skills/
  swift-concurrency/
    SKILL.md - Async/await, Tasks, Actors, structured concurrency
  swiftui-architecture/
    SKILL.md - State management, data flow, app architecture
  navigation/
    SKILL.md - NavigationStack, deep linking, navigation patterns
  testing-di/
    SKILL.md - Unit testing, dependency injection, testing async
  code-review-refactoring/
    SKILL.md - Code smells, refactoring patterns, review guidelines
  performance/
    SKILL.md - SwiftUI performance, memory management, profiling
```
<!-- END REFERENCE STRUCTURE -->

## What These Skills Offer

These skills give your AI coding tool practical Swift guidance:

### Guide Your Swift Decisions
- Choose the right concurrency primitives (async/await, Task, Actor)
- Select appropriate property wrappers (@State, @Binding, @Observable)
- Design navigation flows with NavigationStack and type-safe routing
- Structure testable code with proper dependency injection

### Write Better Swift Code
- Implement structured concurrency patterns safely
- Build performant SwiftUI views with proper state management
- Handle cancellation and errors in async operations
- Create maintainable, testable architectures

### Improve Code Quality
- Identify and fix code smells
- Apply refactoring patterns effectively
- Optimize SwiftUI performance
- Follow iOS best practices

## 🤝 Contributing

These skills are designed for AI agents and human developers. Contributions welcome! This repository follows the [Agent Skills open format](https://agentskills.io/home), which has specific structural requirements.

When contributing:
- Follow the existing SKILL.md format with frontmatter
- Include practical Swift code examples
- Provide clear tradeoffs and decision guidance
- Keep content focused and actionable

## License

This repository is open-source and available under the MIT License.