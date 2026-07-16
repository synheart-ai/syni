# SDK Overview

Syni's SDKs are **thin platform shells** over a shared native core. Business logic — presets, prompt construction, grammar-constrained decoding, schema validation — lives in `syni-runtime` (native, C ABI). The SDKs do platform plumbing only.

## Two SDK families, one app

A typical consumer app uses **both**:

| Family | Role | Produces / Consumes |
|---|---|---|
| `synheart-core-*` (Flutter / Kotlin / Swift) | wraps Synheart Core (storage, sync, crypto, consent) + the Synheart engine | **produces** HSI |
| `syni-*` (Flutter / Kotlin / Swift) | wraps `syni-runtime` (local LLM) and calls Syni Cloud | **consumes** HSI |

The Synheart Core SDKs hand HSI payloads to Syni; Syni runs the persona engine.

## Syni's building blocks

- **Persona spec** (`concepts/personas.md`, sourced from `syni-spec/personas/`)
- **Safety model** (`concepts/safety-model.md`, authoritative in `syni-spec/safety/`)
- **HSI input contract** (`concepts/hsi.md`, defined by `hsi/`)
- **Persona engine pipeline** (`concepts/persona-engine.md`)
- **Cloud gateway** — Syni Cloud

## A minimal interface (conceptual)

```
engine = SyniEngine(persona_id, preset)            // syni-runtime
response = engine.respond({
  user_text,
  hsi_payload,        // from synheart-core-*
  memory,             // local cache + cloud-synced
  execution_mode,     // local | cloud | auto
})
// -> { message, tags, safety, suggestions, ... }
```

Where:

- `persona_id` resolves against `syni-spec/personas/`
- `preset` selects a latency/token budget (`keyboard`, `coach`, `chat`)
- `hsi_payload` is a validated HSI 1.3 document (Syni never reads raw signals)
- `execution_mode` decides local vs. cloud (with auto-routing by latency / privacy tier / complexity)

## What you standardize across SDKs

- **Structured output** — typed responses validated against `syni-runtime/schemas/` (`chat_response`, `coach_response`, `suggestions`, …) even when UI shows plain text.
- **Safety events** — risk level + escalation actions, audit-friendly.
- **Persona + spec versioning** — clients pin a `syni-spec` version per release; Syni Cloud may hot-load minor updates within compat rules.
- **Privacy tier on outbound payloads** — declared explicitly per request; rejected by Syni Cloud if it exceeds user consent.

## Where things live

| If you're working on… | Look in… |
|---|---|
| inference engine, presets, prompt builder, grammars | `syni-runtime` |
| persona, safety, rules, budgets, grammars (spec) | `syni-spec` |
| cloud orchestration, LLM router, memory | Syni Cloud |
| HSI producer / storage / consent | the Synheart engine, Synheart Core |
| Flutter / Kotlin / Swift bindings | `syni-flutter`, `syni-kotlin`, `syni-swift` |
| Dev / CLI testing | the Syni dev CLI |
