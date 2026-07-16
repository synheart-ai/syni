# Persona Engine

The **persona engine** is the runtime that takes a persona definition and produces *consistent, safe, and context-aware* outputs.

At a minimum it composes:

- **Persona**: identity, tone, goals, boundaries (`concepts/personas.md`) — sourced from `syni-spec/personas/`, materialized in Syni Cloud.
- **Context**: the **HSI payload** (`concepts/hsi.md`) plus conversation + environment, framed via CCO (`concepts/cco.md`). HSI is produced upstream by the Synheart engine and arrives as a typed input.
- **Policies**: safety constraints + escalation (`concepts/safety-model.md`), authoritative in `syni-spec/safety/` and `syni-spec/rules/`.
- **Execution**: model/tool routing (local `syni-runtime` vs. Syni Cloud), retries, and output shaping (`concepts/ppe.md`).

## Responsibilities

- **Consistency**: keep voice and boundaries stable over time.
- **Grounding**: attach the right context (signals, history) without flooding the model.
- **Safety**: enforce “must not” behaviors and trigger escalation when needed.
- **Determinism where it matters**: stable formatting for UI + tool calls; probabilistic where creativity helps.

## A reference pipeline

1. **Input normalization**
   - user message
   - conversation state
   - **HSI payload** (validated, consent-gated; produced by the Synheart engine)
2. **Risk + intent classification**
   - detect self-harm risk, medical/legal, harassment, etc.
   - detect user intent (plan, reflect, vent, ask for steps)
3. **Context assembly**
   - concise summary of relevant history
   - relevant HSI axes (e.g. `affective`, `cognitive`) summarized with confidence
   - environmental hints (time, task, app context) from HSI's `digital` axis or app-level state
4. **Prompt composition**
   - persona header (role, style, boundaries)
   - policy header (safety + escalation)
   - CCO frame (Context / Constraints / Output)
5. **Generation**
   - local `syni-runtime` (GBNF-constrained, preset-bounded) or Syni Cloud LLM router (`concepts/on-device-vs-cloud.md`)
6. **Post-processing**
   - validate against `syni-runtime/schemas/` (`chat_response`, `coach_response`, `suggestions`, …)
   - repair → fallback if schema invalid
   - redact disallowed content; enforce length / tone constraints
7. **Logging + learning hooks**
   - capture anonymized metrics
   - store “what worked” signals for iteration (research workflows)

## Outputs

The engine should produce *structured outputs* even if you display them as plain text.

Example (conceptual):

- `assistant_message`: what user sees
- `tags`: `["focus","gentle","actionable"]`
- `safety`: `{ level: "low" | "medium" | "high", actions: [...] }`
- `next_step_suggestions`: UI chips / quick actions

## Failure modes to design for

- **Persona drift**: tone changes across turns → enforce persona header every turn + style tests.
- **Context overload**: too many signals/history → summarize + rank relevance.
- **False safety triggers**: over-escalation → calibrate thresholds and add “confirm intent” steps.
- **Unsafe helpfulness**: agent tries to “solve” harmful requests → hard refusals + safe redirection.

