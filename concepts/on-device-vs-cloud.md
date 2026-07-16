# On-device vs Cloud

Syni can run in two modes. You can also run a hybrid: do safety + lightweight coaching on-device, and escalate to cloud for deeper reasoning or tool use (with consent).

In both modes, **HSI is produced upstream by the Synheart engine** — the on-device / cloud distinction here is purely about *where the LLM runs*, not about where state is inferred.

## On-device (`syni-runtime`)

**Best for**: privacy, low latency, offline use.

Runs in-process via the `syni-runtime` C ABI (llama.cpp with a native control-plane, GBNF-constrained decoding, preset-bounded budgets).

- **Pros**
  - keeps sensitive context local
  - predictable latency
  - simpler compliance surface
- **Cons**
  - smaller models (lower capability)
  - limited tool access
  - harder to audit across a fleet

## Cloud (Syni Cloud)

**Best for**: richer tools, larger models, team-wide iteration.

Calls cross the network to Syni Cloud, which orchestrates persona materialization, long-term memory, and the LLM router (major hosted providers plus hosted-local). Clients never speak to providers directly.

- **Pros**
  - stronger models + retrieval/tools
  - centralized safety controls + audit
  - easier experimentation (A/B, prompt iteration)
- **Cons**
  - higher privacy risk
  - network dependency
  - operational complexity

## Decision checklist

- **User trust requirements**: do users expect local-only?
- **Data sensitivity**: biometrics, mental health content → bias toward on-device.
- **Capability needs**: complex planning, multi-step tool use → cloud may help.
- **Audit needs**: regulated flows → cloud gateway with strong logging/redaction.

## Hybrid pattern (recommended default)

1. `syni-runtime` handles:
   - basic coaching against HSI
   - first-pass safety detection
   - latency-critical surfaces (keyboard, quick suggestions)
2. Syni Cloud handles (with consent):
   - heavy reasoning
   - retrieval (knowledge base)
   - tool orchestration
   - persona evolution + long-term memory
3. Safety model is enforced in both places (`concepts/safety-model.md`); both conform to `syni-spec`.

(Note: state *inference* — turning sensor signals into HSI — is never part of this decision. That always runs on-device in the Synheart engine. The hybrid choice is only about where the LLM lives.)

