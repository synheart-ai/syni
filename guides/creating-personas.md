# Creating Personas

Personas work when they are **specific, bounded, and testable**.

Personas are authored as **specs in `syni-spec/personas/`** (typically `research/` first, then promoted to `prod/`). At runtime they are materialized per-user by Syni Cloud. Do not hardcode persona strings in clients — load by ID from the spec.

## A persona worksheet

### Identity

- **Name**
- **Role**
- **Audience**

### Voice

- tone (warm, direct, playful, clinical)
- pacing (short, medium, long)
- reading level

### Boundaries

- what it refuses outright
- what it handles with disclaimers
- what it never stores/infers

### Goals

- “north star” outcome
- what “good help” looks like in one turn

### Interaction style

- asks questions vs gives steps
- uses frameworks (CBT, MI, coaching)
- how it handles uncertainty

### Escalation

- trigger phrases / patterns
- risk thresholds (low/med/high)
- what the UI should do on escalation

## Style tests (high leverage)

Write a handful of test prompts and check:

- tone matches persona
- length is appropriate
- boundaries are held
- it does not drift into other roles

## Boundary tests (safety)

Use prompts that:

- request disallowed content
- include self-harm ideation
- include medical/legal requests

The expected behavior is defined by `concepts/safety-model.md`, with authoritative rules in `syni-spec/safety/`. Persona changes MUST pass `syni-spec/conformance/` before promotion to `prod/`.

## Examples

The five production personas — `focus.coach.v1`, `stress.coach.v1`,
`cognitive.companion.v1`, `performance.coach.v1`, `wellness.guide.v1` — ship in the
Syni Spec. See [docs.synheart.ai/syni-spec](https://docs.synheart.ai/syni-spec/overview).

