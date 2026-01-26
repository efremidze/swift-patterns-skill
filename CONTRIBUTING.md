# Contributing to Swift Skills

Thank you for your interest in contributing to Swift Skills! This repository follows the [Agent Skills open format](https://agentskills.io/home) to provide AI coding assistants with practical Swift guidance.

## Repository Structure

This repository contains a single comprehensive skill with multiple reference documents:

```
swift-agent-skill/
├── SKILL.md          # Main workflow, decision trees, and quick reference
└── references/       # Detailed documentation for each topic area
    ├── concurrency.md
    ├── state.md
    ├── navigation.md
    ├── testing-di.md
    ├── performance.md
    └── code-review-refactoring.md
```

## How to Contribute

## Recommended Workflow (Skill Creator)
If you use the `skill-creator` skill, you can:
- Propose changes in plain language
- Have the skill generate or refine `SKILL.md` and reference content
- Review for SwiftUI accuracy and consistency

### Improving the Main Skill (SKILL.md)

The main `SKILL.md` file provides:
- Overview and workflow decision trees
- Core guidelines across all topics
- Quick decision guide to find the right reference
- Cross-topic usage examples

When editing `SKILL.md`:
1. Maintain the frontmatter with name and description
2. Keep the Workflow Decision Tree focused on high-level decisions
3. Add cross-references to appropriate reference files
4. Keep Core Guidelines concise (details belong in references)

### Improving Reference Documents

Each reference file in `references/` covers a specific topic in depth:

1. **Fork the repository** and create a new branch for your changes
2. **Edit the reference file** in the `references/` directory
3. **Maintain the structure**:
   - Title and Overview section
   - Workflow Decision Tree (Review/Improve/Implement)
   - Core Guidelines
   - Tradeoffs
   - Output Template with code examples
   - Common Patterns (if applicable)
   - Anti-Patterns to Avoid
4. **Test your changes** by using the skill with your AI assistant
5. **Submit a pull request** with a clear description of your improvements

### Adding New Reference Topics

To add a new reference topic:

1. **Create a new file** in `references/` with a descriptive name (e.g., `networking-patterns.md`)
2. **Follow the standard reference format**:

```markdown
# Topic Name

## Overview
Brief explanation of the topic

## Workflow Decision Tree
### 1) Review existing code
### 2) Improve existing code  
### 3) Implement new feature

## Core Guidelines
Key best practices

## Tradeoffs
When to use different approaches

## Output Template
```swift
// Practical code examples
```

## Common Patterns
Established patterns for this topic

## Anti-Patterns to Avoid
Common mistakes and how to fix them
```

3. **Update SKILL.md** to reference your new topic:
   - Add to Workflow Decision Tree
   - Add to "When to Use Which Reference" section
   - Add to Quick Decision Guide
   - Add to Reference Files list

4. **Update README.md** to mention the new reference topic

5. **Submit a pull request** explaining the new topic and its use cases

## Quality Standards

### Code Examples
- Use realistic, practical Swift code
- Include comments explaining key concepts
- Show both good and bad examples (anti-patterns)
- Test code for syntax errors

### Writing Style
- Be concise and actionable
- Focus on "when" and "why", not just "what"
- Provide clear tradeoffs for different approaches
- Avoid opinionated architecture choices unless well-justified

### Skill Structure
- Follow the Agent Skills open format
- Include proper frontmatter metadata in SKILL.md only
- Organize content with clear sections
- Make content scannable for AI assistants

## Pull Request Process

1. **Ensure your changes** follow the structure and quality standards
2. **Update documentation** if you're changing structure or adding topics
3. **Describe your changes** clearly in the PR description
4. **Be responsive** to feedback and requested changes
5. **Wait for review** from maintainers before merging

## Content Guidelines

### What to Include
✅ Modern Swift and SwiftUI best practices  
✅ Clear tradeoffs between different approaches  
✅ Practical code examples that can be adapted  
✅ Common patterns and anti-patterns  
✅ When to use specific APIs or patterns  

### What to Avoid
❌ Highly opinionated architectural choices  
❌ Framework-specific advice (unless part of the topic)  
❌ Deprecated APIs without migration guidance  
❌ Complex examples without explanation  
❌ Personal preferences without technical justification  

## Testing Your Changes

Before submitting, test your changes with an AI assistant:

1. **Load the skill** in your AI coding tool
2. **Ask it to use the skill** for a relevant task
3. **Verify** the AI references your changes correctly
4. **Check** that code examples are syntactically correct
5. **Ensure** the guidance is clear and actionable

## Questions?

If you have questions about contributing, please open an issue for discussion before starting major work.

## Code of Conduct

Be respectful and constructive in all interactions. We're here to help each other write better Swift code.

Thank you for helping make Swift Skills better! 🎉
