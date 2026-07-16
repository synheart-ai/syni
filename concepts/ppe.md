# PPE (Persona–Policy–Execution)

PPE is a design separation that keeps systems safer and easier to maintain:

- **Persona**: identity + voice + goals (`concepts/personas.md`)
- **Policy**: safety + boundaries + escalation (`concepts/safety-model.md`)
- **Execution**: runtime choices (model routing, tools, output validation)

## Why PPE matters

- **Prevents prompt soup**: you can evolve persona without weakening safety.
- **Enables testing**: you can test policy and persona separately.
- **Supports hybrid**: on-device persona + local policy checks + cloud execution when needed.

## Example mapping

- Persona says: “Be warm, concise, supportive.”
- Policy says: “Refuse self-harm instructions; escalate on high risk.”
- Execution says:
  - “Use on-device model for short coaching.”
  - “If user requests deep planning, call cloud gateway.”
  - “Validate output contains a single next step + resource link on escalation.”

## Operationalizing PPE

- Store persona definitions as versioned specs.
- Store policy rules as explicit, reviewable documents (and code/config where applicable).
- Treat execution as an implementation detail that can change without changing user expectations.

