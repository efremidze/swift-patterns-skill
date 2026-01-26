# Pitfalls Research

**Domain:** Swift/SwiftUI agent skill for refactor/review guidance
**Researched:** 2026-01-25
**Confidence:** MEDIUM

## Critical Pitfalls

### Pitfall 1: Mixing SwiftUI guidance with non-SwiftUI concerns

**What goes wrong:**
The skill drifts into UIKit, server-side Swift, or general Swift patterns, diluting the SwiftUI-focused guidance and causing agents to recommend irrelevant changes.

**Why it happens:**
Broad prompts and generic refactor templates encourage inclusion of common Swift patterns not specific to SwiftUI.

**How to avoid:**
Define strict scope boundaries up front and enforce them in every section; add a short “in-scope vs out-of-scope” checklist to each guidance area.

**Warning signs:**
Guidance includes UIKit lifecycle, controllers, networking stacks, or server patterns; examples not using SwiftUI views and modifiers.

**Phase to address:**
Phase 1: Scope + constraints definition

---

### Pitfall 2: Architecture mandates disguised as best practices

**What goes wrong:**
The skill prescribes MVVM/Coordinator/DI frameworks and folder structures, violating project constraints and limiting agent applicability.

**Why it happens:**
Content authors default to common architecture narratives, especially in refactor/review guidance.

**How to avoid:**
Replace mandates with neutral guidance (“consider separating business logic for testability”) and explicitly flag architecture choices as out of scope.

**Warning signs:**
“Must use MVVM,” “place ViewModels in X folder,” or templates that assume a specific DI or routing system.

**Phase to address:**
Phase 2: Guidance style rules + constraint compliance

---

### Pitfall 3: Outdated or deprecated SwiftUI API guidance

**What goes wrong:**
The skill recommends deprecated APIs (e.g., older navigation or styling APIs), leading to incorrect refactors and reviews.

**Why it happens:**
Static knowledge baked into guidance; lack of verification against current SwiftUI APIs.

**How to avoid:**
Add a “modern API replacements” section and a validation checklist that requires verifying deprecations against current docs before finalizing guidance.

**Warning signs:**
Instructions use `NavigationView` or `foregroundColor` as preferred defaults without mention of modern replacements.

**Phase to address:**
Phase 3: SwiftUI API currency + modernization pass

---

### Pitfall 4: Treating optional optimizations as requirements

**What goes wrong:**
The skill mandates performance optimizations (e.g., downsampling, Equatable views) even when not needed, leading to over-engineering.

**Why it happens:**
Refactor/review checklists are often copied from performance-focused guidelines without context.

**How to avoid:**
Label optimizations as “consider” and tie them to observed symptoms; include “only when needed” criteria.

**Warning signs:**
Language like “always” on optional optimizations; no condition or trigger described.

**Phase to address:**
Phase 3: Performance guidance rules

---

### Pitfall 5: Contradictory instructions across files

**What goes wrong:**
Different docs conflict on rules (e.g., one says avoid concurrency guidance, another includes it), confusing downstream agents.

**Why it happens:**
Multiple authors or templates without a single source of truth.

**How to avoid:**
Centralize constraints in a single authoritative section and reference it from all other docs; add a conflict review step.

**Warning signs:**
Duplicate rules or mismatched phrasing for allowed/disallowed topics; repeated “exceptions” that disagree.

**Phase to address:**
Phase 4: Consistency audit + conflict resolution

---

### Pitfall 6: Missing decision logic for refactor vs review

**What goes wrong:**
Agents apply refactor guidance when they should only review, or skip actionable refactors during review.

**Why it happens:**
Guidance focuses on content but not workflow and decision triggers.

**How to avoid:**
Add a decision tree: detect task intent (review vs refactor) and map to allowed actions and output format.

**Warning signs:**
Guidelines are purely descriptive with no flow or branching; same checklist used for both refactor and review.

**Phase to address:**
Phase 2: Workflow + decision logic

---

### Pitfall 7: Unverifiable claims presented as facts

**What goes wrong:**
The skill asserts specific SwiftUI behaviors without verification, leading to incorrect recommendations.

**Why it happens:**
Relying on training data or memory instead of current sources.

**How to avoid:**
Require citations or explicit confidence labels for claims about API behavior or new features; include a “verify before assert” rule.

**Warning signs:**
Statements like “always” or “never” about SwiftUI behavior without sources or caveats.

**Phase to address:**
Phase 3: Verification rules + evidence tagging

---

## Technical Debt Patterns

Shortcuts that seem reasonable but create long-term problems.

| Shortcut | Immediate Benefit | Long-term Cost | When Acceptable |
|----------|-------------------|----------------|-----------------|
| One monolithic “best practices” doc | Fast initial delivery | Becomes unscannable; conflicts increase | MVP only, if split plan is scheduled | 
| Copying generic Swift checklists | Quick content fill | Violates SwiftUI scope and constraints | Never | 
| Hardcoding single architecture examples | Clear examples | Implies mandates, reduces applicability | Only if clearly labeled “example, not requirement” |

## Integration Gotchas

Common mistakes when connecting to external services.

| Integration | Common Mistake | Correct Approach |
|-------------|----------------|------------------|
| Orchestrator triggers (GSD) | Missing structured return format | Always include required return block and fields | 
| Skills loader / agent registry | Missing or incorrect frontmatter/tool list | Validate against agent metadata schema | 
| Template reuse | Editing the template file instead of instantiating into `.planning/research/` | Copy template content into target file only |

## Performance Traps

Patterns that work at small scale but fail as usage grows.

| Trap | Symptoms | Prevention | When It Breaks |
|------|----------|------------|----------------|
| Overlong, dense guidance docs | Agents miss constraints or skip sections | Split by task type with short summaries | When docs exceed ~8–10 pages or ~6–8k tokens | 
| No “at-a-glance” rules | Agents ignore nuanced guidance | Provide a concise “rules of engagement” section | When task turnaround is short or many agents involved | 
| Duplicated rules across files | Conflicts and drift | Single source of truth with references | When team updates are frequent |

## Security Mistakes

Domain-specific security issues beyond general web security.

| Mistake | Risk | Prevention |
|---------|------|------------|
| Including real secrets or credentials in examples | Leakage through agent outputs | Use redacted placeholders and explicit “do not include secrets” rule |
| Encouraging destructive git commands | Data loss in agent-driven workflows | Explicitly forbid hard resets and force pushes unless user requests |
| Instructing tool-specific steps that require elevated access | Unauthorized operations | Keep tool usage generic and safe by default |

## UX Pitfalls

Common user experience mistakes in this domain.

| Pitfall | User Impact | Better Approach |
|---------|-------------|-----------------|
| Ambiguous “do X” with no decision criteria | Inconsistent agent outputs | Add decision logic and examples of when to apply | 
| Advice framed as mandates without scope | Agents over-apply guidance | Use conditional language and scope markers | 
| Missing “what to say to user” guidance | Low-signal responses | Provide response structure and brevity rules |

## "Looks Done But Isn't" Checklist

Things that appear complete but are missing critical pieces.

- [ ] **Decision logic:** Often missing a review vs refactor split — verify a task-intent decision tree exists.
- [ ] **Scope guardrails:** Often missing explicit exclusions (UIKit, server-side Swift) — verify scope section is present.
- [ ] **Modern API guidance:** Often missing deprecation replacements — verify a “use X instead of Y” list exists.
- [ ] **Consistency rules:** Often missing conflict resolution — verify one authoritative constraints section is referenced everywhere.

## Recovery Strategies

When pitfalls occur despite prevention, how to recover.

| Pitfall | Recovery Cost | Recovery Steps |
|---------|---------------|----------------|
| Scope creep into non-SwiftUI topics | MEDIUM | Remove out-of-scope content, add scope gate, rerun consistency review |
| Architecture mandates embedded | MEDIUM | Rewrite mandates into optional guidance, add “not required” labels |
| Deprecated API recommendations | HIGH | Audit all API mentions, update to modern equivalents, add verification checklist |

## Pitfall-to-Phase Mapping

How roadmap phases should address these pitfalls.

| Pitfall | Prevention Phase | Verification |
|---------|------------------|--------------|
| Mixing SwiftUI with non-SwiftUI | Phase 1: Scope + constraints | Scope checklist applied to all docs |
| Architecture mandates | Phase 2: Guidance style rules | No “must use MVVM/DI/routers” language | 
| Deprecated API guidance | Phase 3: API currency pass | Modern replacements list reviewed | 
| Optional optimizations as mandates | Phase 3: Performance guidance | “Consider/when” language used | 
| Contradictory instructions | Phase 4: Consistency audit | Single source of truth referenced | 
| Missing decision logic | Phase 2: Workflow definition | Decision tree included and tested |
| Unverifiable claims | Phase 3: Verification rules | Confidence labels and sources present |

## Sources

- Repository constraints: `AGENTS.md`
- Project instructions: `.opencode/agents/gsd-project-researcher.md`
- Project templates: `.opencode/get-shit-done/templates/research-project/PITFALLS.md`

---
*Pitfalls research for: Swift/SwiftUI agent skill refactor/review guidance*
*Researched: 2026-01-25*
