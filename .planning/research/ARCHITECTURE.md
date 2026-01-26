# Architecture Research

**Domain:** Agent Skill architecture for Swift/SwiftUI refactor + review guidance
**Researched:** 2026-01-25
**Confidence:** LOW

## Standard Architecture

### System Overview

```
┌───────────────────────────────────────────────────────────────────────┐
│                         Skill Interface Layer                         │
├───────────────────────────────────────────────────────────────────────┤
│  ┌────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐ │
│  │ Prompts &  │  │ Output Rules │  │ Glossary &   │  │ Constraints  │ │
│  │ Intents    │  │ & Templates  │  │ Terminology  │  │ (Do/Don't)   │ │
│  └─────┬──────┘  └─────┬────────┘  └─────┬────────┘  └─────┬────────┘ │
│        │               │                │                │           │
├────────┴───────────────┴────────────────┴────────────────┴───────────┤
│                         Decision Logic Layer                          │
├───────────────────────────────────────────────────────────────────────┤
│  ┌───────────────────────────────────────────────────────────────┐    │
│  │  Triage → Scope → Workflow Selection → Decision Gates          │    │
│  └───────────────────────────────────────────────────────────────┘    │
├───────────────────────────────────────────────────────────────────────┤
│                         Workflow Modules                              │
├───────────────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐                │
│  │ Refactor     │  │ Review       │  │ Diagnostics  │                │
│  │ Playbooks    │  │ Playbooks    │  │ & Escalation │                │
│  └──────────────┘  └──────────────┘  └──────────────┘                │
├───────────────────────────────────────────────────────────────────────┤
│                         Knowledge Units                               │
├───────────────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐                │
│  │ SwiftUI      │  │ Concurrency  │  │ Navigation   │                │
│  │ State Rules  │  │ & Testing    │  │ & Lists      │                │
│  └──────────────┘  └──────────────┘  └──────────────┘                │
└───────────────────────────────────────────────────────────────────────┘
```

### Component Responsibilities

| Component | Responsibility | Typical Implementation |
|-----------|----------------|------------------------|
| Prompts & intents | Define entry points and expected outcomes (refactor vs review) | Short intent blocks and role framing in `README`/entry docs |
| Constraints | Enforce domain boundaries (no architecture mandates, no tooling steps) | Do/Don't lists with rationale |
| Output rules & templates | Keep responses consistent and high-signal | Response schema + example outputs |
| Glossary & terminology | Normalize language across guidance | Definitions and preferred terms |
| Decision logic | Route requests to correct workflow and gates | Triage checklist + decision tables |
| Refactor playbooks | Step-by-step guidance for refactor tasks | Task flows, decision gates, examples |
| Review playbooks | Code review checklists and findings taxonomy | Issue categories, severity, remediation |
| Diagnostics & escalation | When to ask questions or defer | Known unknowns, validation triggers |
| Knowledge units | Swift/SwiftUI rules and pitfalls | Modular topic notes and examples |

## Recommended Project Structure

```
.opencode/
├── agents/                         # Agent roles and high-level behavior
├── get-shit-done/                  # Orchestrator + templates
└── skills/                         # Skill content root
    └── swiftui-refactor-review/    # Skill package
        ├── INDEX.md                # Entry point + navigation
        ├── CONSTRAINTS.md          # Do/Don't + exclusions
        ├── OUTPUT.md               # Response format templates
        ├── GLOSSARY.md             # Domain terms
        ├── DECISIONS.md            # Triage + routing logic
        ├── workflows/              # Refactor/review playbooks
        │   ├── REFACTOR.md
        │   └── REVIEW.md
        ├── topics/                 # Modular knowledge units
        │   ├── STATE.md
        │   ├── CONCURRENCY.md
        │   ├── NAVIGATION.md
        │   ├── PERFORMANCE.md
        │   └── TESTING_DI.md
        └── examples/               # Minimal, focused examples
            ├── refactor-patterns.md
            └── review-findings.md
```

### Structure Rationale

- **`DECISIONS.md`:** separates routing logic from content so workflow selection stays stable as topic content grows.
- **`workflows/`:** refactor and review flows remain distinct but can link into shared topic modules.
- **`topics/`:** small, reusable units avoid repetition and allow partial updates without touching workflows.

## Architectural Patterns

### Pattern 1: Decision-Gated Guidance

**What:** Route requests through gates (scope, change type, severity) before guidance.
**When to use:** Any request that could be review or refactor, or risks violating constraints.
**Trade-offs:** Adds upfront steps but prevents mis-scoped advice.

**Example:**
```markdown
Gate 1: Is the request a review or refactor?
Gate 2: Is the change SwiftUI view, state, navigation, or concurrency?
Gate 3: Does it require asking a question (blocking info)?
```

### Pattern 2: Atomic Rules + Composite Workflows

**What:** Keep canonical rules in topic modules; workflows reference them.
**When to use:** Domains with overlapping guidance across multiple workflows.
**Trade-offs:** Requires linking discipline, but reduces drift.

**Example:**
```markdown
Workflow step → link to topics/STATE.md#ForEach-identity
```

### Pattern 3: Findings Taxonomy for Reviews

**What:** Classify review issues by type and severity (correctness, performance, maintainability, accessibility).
**When to use:** Review output must be consistent across different codebases.
**Trade-offs:** Slightly more structure, faster consumption.

## Data Flow

### Request Flow

```
User Request
    ↓
Triage (intent + scope)
    ↓
Decision Gates (constraints + topic routing)
    ↓
Workflow Module (refactor or review)
    ↓
Topic Units (state, navigation, concurrency, performance)
    ↓
Output Template (final response)
```

### State Management

```
Knowledge Units → Workflows → Output
      ↑                 ↓
   Constraints ← Decision Logic
```

### Key Data Flows

1. **Refactor request:** intent → refactor workflow → topic references → recommendations → output.
2. **Review request:** intent → review workflow → findings taxonomy → remediation → output.

## Scaling Considerations

| Scale | Architecture Adjustments |
|-------|--------------------------|
| Small skill | Keep all rules in one file to reduce navigation friction |
| Medium skill | Split by workflows + topics; add decision logic file |
| Large skill | Add cross-topic index, tags, and change log for updates |

### Scaling Priorities

1. **First bottleneck:** conflicting guidance across files → fix with single-source topic units.
2. **Second bottleneck:** inconsistent outputs → fix with output templates and examples.

## Anti-Patterns

### Anti-Pattern 1: Workflow-Only Guidance

**What people do:** Put all rules inside workflows.
**Why it's wrong:** Rules drift, repeated edits, inconsistent advice.
**Do this instead:** Keep atomic rules in topic units and reference them.

### Anti-Pattern 2: Monolithic Knowledge Dump

**What people do:** One giant document for everything.
**Why it's wrong:** Hard to keep current; weak routing; slow to update.
**Do this instead:** Split into decisions, workflows, and topic modules.

## Integration Points

### External Services

| Service | Integration Pattern | Notes |
|---------|---------------------|-------|
| None (skill-only) | N/A | Avoid tool-specific steps per project constraints |

### Internal Boundaries

| Boundary | Communication | Notes |
|----------|---------------|-------|
| Decision logic ↔ workflows | References and links | Keep routing in one place |
| Workflows ↔ topics | Direct references | Avoid duplicating rules |

## Build Order (Dependencies)

1. **Constraints + glossary** → establishes boundaries and terms for all other files.
2. **Decision logic** → enables routing to refactor vs review.
3. **Workflows** → refactor and review playbooks.
4. **Topic units** → attach as references from workflows.
5. **Output templates + examples** → validate guidance consistency.

## Sources

- No external sources used; based on internal synthesis. Confidence is LOW and should be validated.

---
*Architecture research for: agent skill for Swift/SwiftUI refactor + review*
*Researched: 2026-01-25*
