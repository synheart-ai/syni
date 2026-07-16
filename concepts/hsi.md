# HSI (Human State Interface)

HSI is **not part of Syni**. It is a versioned JSON contract for human-state
outputs, defined in the `hsi/` repository. Syni *consumes* HSI; Syni does not
*produce* it.

> **Mental model:** HSI is to human-state data what JSON is to structured data,
> or what GPS is to location — a shared interface that many systems can implement.

## What HSI is

- A **standardized payload contract** (canonical schemas `hsi-1.0` … `hsi-1.3`)
- Five canonical axes (HSI 1.3, RFC-HSI-0010):
  - `physiological` — HR, HRV, recovery, strain, …
  - `kinematic` — motion, posture, activity
  - `digital` — interaction, app context, behavioral signals
  - `cognitive` — focus, capacity, load
  - `affective` — emotion, arousal, valence
- Per-axis readings with **modality attribution** and optional
  `confidence_breakdown` (RFC-HSI-0011)
- Forward-compatible: unknown fields MUST be tolerated by consumers

## What HSI is not

- Not an SDK or runtime
- Not a model, dataset, or training recipe
- Not a sensor-acquisition standard
- Not tied to any single producer

## Who produces HSI

The reference producer in this ecosystem is **the Synheart engine**:

```
signals -> session-runtime -> state-runtime -> flux -> HSI payload
```

Storage, sync, crypto, and consent enforcement of HSI live in
**Synheart Core** (and its platform SDKs `synheart-core-flutter`,
`-kotlin`, `-swift`).

Other producers (third-party wearables, behavioral SDKs) can emit valid HSI
without using Synheart's engine — that is the whole point of a contract.

## How Syni uses HSI

Syni receives a validated HSI payload and uses it as **conditioning** for the
persona engine. Specifically:

- The local `syni-runtime` `PromptBuilder` reads the HSI payload and composes
  HSI-aware prompts.
- Syni Cloud accepts HSI (or a reduced summary, depending on
  consent tier) and forwards conditioned prompts to upstream LLM providers.
- Policy rules in `syni-spec/rules/` map HSI state to recommended
  persona behaviors deterministically.

Syni MUST NOT:

- bypass the contract to read raw signals,
- assume any specific producer or model,
- store or transmit anything beyond what the consent layer permits.

## Principles for Syni's HSI use

- **Treat readings as estimates**, not facts. Surface confidence; do not claim
  "you are anxious."
- **Honor consent tiers** declared by Synheart Core. Syni does not
  re-implement consent.
- **Ask for confirmation** when axis confidence is low.
- **Avoid overfitting** to a single bad reading; rely on trends or recent
  windows where appropriate.
- **Explainability**: if HSI influenced behavior, the UI SHOULD be able to
  surface *why*.

## Extending what Syni can see

If Syni needs a new dimension of human state, the correct path is to extend
HSI itself — file an RFC against `hsi/`. Do not add side-channel inputs that
bypass the contract; that breaks privacy, portability, and conformance.

## See also

- [`hsi`](https://github.com/synheart-ai/hsi) — canonical schemas + RFCs (HSI as an input contract)
- `concepts/persona-engine.md` — how HSI feeds prompt composition
- `concepts/cco.md` — using HSI as the *Context* in CCO framing
