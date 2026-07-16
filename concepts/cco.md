# CCO (Context, Constraints, Output)

CCO is a simple pattern for making agent behavior predictable:

- **Context**: what the agent should consider
- **Constraints**: what it must/must not do
- **Output**: the exact shape of what it should produce

CCO is especially useful when:

- you want consistent formatting for a UI
- you want predictable step-by-step coaching
- you want safety constraints enforced every turn

## Template (copy/paste)

**Context**

- HSI snapshot: relevant axes (e.g. `affective.valence`, `cognitive.focus`) with confidence
- Recent history: …
- Current goal: …

**Constraints**

- Persona boundaries: …
- Safety rules: …
- Style rules (tone, length): …

**Output**

- Provide: …
- Format: …
- Include: …
- Avoid: …

## Example: Focus Coach

**Context**

- User is procrastinating on a work task and feels overwhelmed.
- Time available: 25 minutes.

**Constraints**

- Be gentle, non-shaming.
- Ask at most one clarifying question.
- Offer a plan with a tiny first step.

**Output**

- 1 short question (optional)
- a 3-step plan
- a “start now” first step under 2 minutes

See also `concepts/persona-engine.md`.

