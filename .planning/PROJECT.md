# Swift Expert Skill Refactor

## What This Is

A refactor of this repository into a reusable Agent Skill that AI coding agents can load on demand for Swift/SwiftUI review and refactoring. It will provide clear workflows, decision logic, and high-signal best-practice guidance as actionable rules and checklists.

## Core Value

AI agents can quickly apply consistent, modern Swift/SwiftUI guidance to refactor and review code without ambiguity.

## Requirements

### Validated

- ✓ Existing Agent Skill content with a workflow entry point — existing (`swift-agent-skill/SKILL.md`)
- ✓ Topic-specific reference docs covering core Swift/SwiftUI areas — existing (`swift-agent-skill/references/*.md`)
- ✓ Repository-level usage documentation — existing (`README.md`)
- ✓ Restructure skill content for immediate agent usability (clear workflows and decision logic) — v1.0
- ✓ Provide actionable rules and checklists for Swift/SwiftUI refactoring and review — v1.0
- ✓ Ensure guidance is modern and agent-friendly (focused on SwiftUI, concurrency, navigation, testing/DI, performance/refactoring) — v1.0

### Active

- [ ] None yet — define during next milestone planning

### Out of Scope

- Architecture mandates — avoid prescribing MVVM/MVC/VIPER or similar
- Formatting/style rules — no linting or ordering requirements
- Tool-specific steps — no Xcode/CLI instructions beyond basics
- Swift language deep dives — avoid non-SwiftUI Swift features

## Context

This repository already contains an Agent Skill in `swift-agent-skill/` with topic references in `swift-agent-skill/references/`, plus lightweight Node.js hooks in `.opencode/hooks/` and `.claude/hooks/`. The refactor will focus on making the skill content immediately usable by AI agents via structured workflows, decision logic, and concise guidance.

## Current State

v1.0 shipped with a complete Swift/SwiftUI agent skill: constraint-linked workflows, decision routing, core SwiftUI guidance, quality/playbooks, and constraint-aligned reference hub content.

## Constraints

- **Format**: Must follow Agent Skills format with metadata and procedural instructions — required for agent loading
- **Audience**: Optimize first for AI coding agents — fast lookup and step-by-step guidance
- **Scope**: Emphasize SwiftUI state, concurrency, navigation, testing/DI, and performance/refactoring
- **Exclusions**: No architecture mandates, formatting rules, or tool-specific instructions

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Optimize for immediate agent usability | Primary goal is agent on-demand usage | ✓ Shipped v1.0 |
| Emphasize SwiftUI state, concurrency, navigation, testing/DI, performance/refactoring | Core areas for refactor/review workflows | ✓ Shipped v1.0 |
| Exclude architecture mandates, formatting rules, tool-specific steps | Aligns with skill guidelines and avoids prescriptions | ✓ Shipped v1.0 |

## Next Milestone Goals

- Define the next set of requirements and roadmap after v1.0 archive

---
*Last updated: 2026-01-26 after v1.0 milestone*
