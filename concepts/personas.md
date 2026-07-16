# Personas

A **persona** is a specification that keeps an experience coherent: *voice, goals, boundaries, and "what good looks like."*

Personas have **two layers**:

- **Spec layer** — declarative, versioned, signed persona definitions in `syni-spec/personas/{core,prod,research}/`. Immutable per version. This is the contract.
- **Runtime layer** — per-user materialization in Syni Cloud: which persona is active, evolved parameters, feedback history, A/B assignment. This is the state.

Personas cannot do anything the spec does not permit.

Syni personas are meant to be:

- **Composable**: combine base traits + scenario-specific traits
- **Testable**: style tests + boundary tests, enforced by `syni-spec/conformance/`
- **Portable**: identical behavior across local `syni-runtime` and Syni Cloud

## Minimal persona schema (recommended)

- **Name**
- **Role**: what it is *for* (coach, companion, assistant, keyboard helper)
- **Audience**: who it serves
- **Voice**: tone, phrasing, pace, reading level
- **Boundaries**
  - topics to refuse
  - topics to handle with care + disclaimers
  - privacy constraints (what not to infer/store)
- **Goals**
  - long-term (e.g., “build sustainable focus habits”)
  - short-term (e.g., “help user plan next 20 minutes”)
- **Interaction style**
  - question/answer ratio
  - whether it uses frameworks (CBT, motivational interviewing)
  - how it handles uncertainty
- **Escalation rules** (can reference `concepts/safety-model.md`)

## Persona design principles

- **Be bounded**: “helpful” without boundaries becomes unsafe.
- **Be legible**: the user should understand what the persona can/can’t do.
- **Be non-coercive**: suggestions, not pressure.
- **Be privacy-respecting**: treat signals as *estimates*, not facts.

## Example persona: “Focus Coach”

- **Role**: practical, gentle focus support
- **Voice**: warm, concise, lightly structured
- **Boundaries**:
  - no medical diagnosis
  - no shaming language
  - avoid productivity absolutism (“always/never”)
- **Goals**:
  - help user choose one next action
  - reduce task avoidance with tiny steps
- **Interaction style**:
  - ask one clarifying question maximum before proposing a plan
  - default to 10–25 minute timeboxes

See `guides/creating-personas.md` for a full persona worksheet.

