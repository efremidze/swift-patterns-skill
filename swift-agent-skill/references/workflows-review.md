# Review Workflow (SwiftUI)

Use this workflow when the user asks for findings, risks, or assessment without code changes.

**Required references:**
- **Constraints (mandatory):** See [SKILL.md Constraints](../SKILL.md#constraints).
- **Invariants (mandatory):** See [Invariants reference](invariants.md).

## Scope and Inputs

Record the scope and inputs before reviewing:

- Files and components reviewed
- Requirements or expected behaviors
- Platform constraints (iOS version, feature flags, availability)

## Findings Taxonomy

Classify findings consistently so review output is stable and comparable:

- **Correctness:** Behavior mismatches, edge cases, crashes, or broken flows
- **Data flow:** Ownership, bindings, and unidirectional flow violations
- **Navigation:** Competing sources of truth, path mismatches, or route handling gaps
- **Identity:** Unstable list identity, incorrect `id:` usage, state loss
- **Performance:** Unnecessary recomputation, heavy work in body, missing lazy containers
- **Accessibility (when required or requested):** Missing labels, contrast, or focus issues

## Review Checklist

- Confirm the request is a review (findings only, no code changes)
- List the files and requirements used for the review
- Check stable identity in `List`/`ForEach` (no `.indices` for dynamic data)
- Verify state ownership mapping (`@State` local, `@Binding` parent-owned, `@Observable` shared)
- Confirm unidirectional data flow (data down, events up)
- Ensure navigation has a single source of truth (one root/path)
- Confirm async work is tied to view lifecycle (`.task` or explicit cancellation)
- Flag deprecated API usage when modern replacements are required
- Call out performance risks when work is heavy in view body or lists lack stable identity

## Risk Cues (Ask for Tests or Split Work)

Escalate when you see any of these signals:

- No tests cover the behavior being reviewed or changed soon after
- State ownership is changing or being inferred during review
- Navigation path state appears in multiple places or is being redefined
- List identity changes could reset selection or row state
- Async work was moved or detached from view lifecycle
- Refactor scope touches multiple screens or core flows without verification
