# Workflow Routing Decisions

Use this reference to route requests to the review or refactor workflow using explicit intent cues. This keeps workflow selection consistent and avoids duplicating constraints.

## Routing Gates

| Gate | Signal | Route | Notes |
| --- | --- | --- | --- |
| Intent cues | "review", "find issues", "audit", "evaluate", "assess" | Review | User wants findings, not code changes. |
| Intent cues | "refactor", "change", "implement", "rewrite", "simplify", "improve" | Refactor | User wants code changes or new behavior. |
| SwiftUI scope | Mentions SwiftUI state, navigation, lists, or view composition | Keep SwiftUI focus | Stay within Swift/SwiftUI guidance. |
| Risk cues | "no tests", "legacy", "critical", "migration" | Refactor with safety checklist | Emphasize behavior preservation and safety gates. |

## Examples (Explicit Intent)

**Review intent (find issues only):**
- "Review this view for SwiftUI state issues and list identity risks."
- "Find code smells and potential bugs in this SwiftUI screen."

**Refactor intent (change or implement):**
- "Refactor this view to improve state ownership and simplify navigation."
- "Implement a safer data flow for this screen and clean up the logic."

## Ambiguous Requests

If the request does not clearly signal review vs refactor intent, ask a direct question before proceeding:

- "Do you want findings only (review), or do you want me to change the code (refactor)?"

## Shared Constraints

All workflows must follow the Constraints section in `swift-expert-skill/SKILL.md`. This is mandatory for both review and refactor responses.
