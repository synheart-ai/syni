# Glossary

## Core concepts

- **Persona**: A versioned, structured definition of an agent's role, voice, boundaries, and goals. Declared in `syni-spec/personas/`, materialized at runtime in Syni Cloud.
- **Persona engine**: Runtime that composes persona + HSI + policies into a schema-safe response.
- **HSI (Human State Interface)**: A versioned JSON **contract** for human-state outputs (`hsi/`). Five canonical axes in HSI 1.3: `physiological`, `kinematic`, `digital`, `cognitive`, `affective`. Syni *consumes* HSI; it does not produce it.
- **CCO**: Context, Constraints, Output — a framing pattern for predictable agent behavior.
- **PPE**: Persona–Policy–Execution — separating tone/identity, safety rules, and runtime behavior.
- **Safety model**: Policies + detectors + escalation paths declared in `syni-spec/safety/`.
- **Preset**: A latency/token/grammar envelope enforced by `syni-runtime` (e.g. `keyboard`, `coach`, `chat`).
- **Escalation**: Shifting from coaching to safety-first actions when risk is detected.

## Runtime / deployment

- **On-device**: Local inference via `syni-runtime` (llama.cpp with a native C ABI); privacy-first, lower latency, constrained models.
- **Cloud gateway**: Syni Cloud — orchestrator, LLM router, persona store, long-term memory. Brokers requests to upstream providers (major hosted LLMs plus hosted-local) with auditability.
- **LLM router**: The component inside Syni Cloud that picks an upstream provider based on latency, cost, privacy tier, plan, and persona safety requirements. Clients MUST NOT call providers directly.

## Components in the Syni ecosystem

- **HSI** (`hsi`): Canonical HSI schemas + RFCs (HSI-0008, HSI-0010, HSI-0011). The input contract.
- **The Synheart engine**: Deterministic on-device engine that *produces* HSI from sensors.
- **Synheart Core**: On-device SDK backend — storage, sync, crypto, consent, capabilities. Source of truth for consent enforcement.
- **`syni-runtime`**: Portable on-device LLM engine — the local half of Syni.
- **`syni-spec`**: Behavioral contract — personas, grammars, rules, safety, budgets, conformance.
- **Syni Cloud**: Cloud gateway — orchestrator, LLM router, persona store, and long-term memory. The cloud half of Syni.
