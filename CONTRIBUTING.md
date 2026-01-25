# Contributing to Swift Skills

Thank you for your interest in contributing to Swift Skills! This repository follows the [Agent Skills open format](https://agentskills.io/home) to provide AI coding assistants with practical Swift guidance.

## How to Contribute

### Improving Existing Skills

1. **Fork the repository** and create a new branch for your changes
2. **Edit the SKILL.md file** in the appropriate skill directory
3. **Maintain the structure**: 
   - Frontmatter with name and description
   - Overview section
   - Workflow Decision Tree
   - Core Guidelines
   - Tradeoffs
   - Output Template with code examples
   - Common Patterns (if applicable)
   - Anti-Patterns to Avoid
4. **Test your changes** by using the skill with your AI assistant
5. **Submit a pull request** with a clear description of your improvements

### Adding New Skills

1. **Create a new directory** with a descriptive name (lowercase, hyphenated)
2. **Add a SKILL.md file** following the standard format:

```markdown
---
name: your-skill-name
description: One-line description of when to use this skill
---

# Your Skill Name

## Overview
Brief explanation of the skill's purpose

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
```

3. **Update README.md** to include your new skill in the appropriate section
4. **Submit a pull request** explaining the new skill and its use cases

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
- Include proper frontmatter metadata
- Organize content with clear sections
- Make content scannable for AI assistants

## Pull Request Process

1. **Ensure your changes** follow the structure and quality standards
2. **Update documentation** if you're changing file locations or adding skills
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
❌ Framework-specific advice (unless that's the skill's focus)  
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
