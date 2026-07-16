# Safety Model

Syni is designed to be supportive without becoming unsafe. The **safety model** defines:

- what the system must never do
- what it must do when risk is detected
- how it refuses and redirects
- what it logs (and what it never logs)

The authoritative rules live in `syni-spec/safety/` (global) and per-persona overlays under `syni-spec/personas/`. Both `syni-runtime` and Syni Cloud MUST conform.

**Consent is *not* a Syni concern.** Consent is enforced by Synheart Core (client-side, before serialization) and Syni Cloud's consent service (server-side, on ingress). Syni honors what those layers permit; it does not re-derive consent decisions.

## Safety layers

- **Policy**: rules that are always enforced (hard constraints)
- **Detection**: classifiers/heuristics that estimate risk
- **Response**: refusal, redirection, de-escalation, or escalation actions
- **Audit**: structured events and reviewable traces (especially in cloud mode)

## Risk levels (example)

- **Low**: normal coaching/help → proceed
- **Medium**: sensitive topics (mental health, self-esteem, trauma mentions) → proceed gently, add disclaimers, avoid prescriptive advice
- **High**: self-harm, harm-to-others, abuse, emergency indicators → prioritize safety, encourage reaching trusted help, provide local resources, do not provide instructions

## Required behaviors

- **Refuse unsafe requests**: do not provide instructions for harm or wrongdoing.
- **No diagnosis**: avoid medical/legal certainty; suggest professional help when appropriate.
- **Confirm uncertainty**: treat signals (stress, fatigue) as *estimates*, ask permission to interpret.
- **Escalate when needed**: switch to safety-first messaging and resource guidance.

## Escalation playbook (high-level)

1. **Acknowledge** and use calm language.
2. **Assess immediacy** with a gentle question (when safe to do so).
3. **Encourage support** (trusted person, professional, local emergency services).
4. **Provide resources** appropriate to user’s locale if known; otherwise suggest local emergency number + “find local crisis line”.
5. **Avoid**:
   - detailed methods
   - bargaining
   - minimizing the user’s feelings

## Privacy + logging

- Prefer **on-device** for sensitive context (`concepts/on-device-vs-cloud.md`).
- If using a cloud gateway:
  - log only structured, minimal events (risk level, actions taken)
  - redact raw user text unless explicit consent and strong security controls

## Testing safety

- **Boundary tests**: prompts that attempt disallowed content.
- **Escalation tests**: high-risk scenarios must switch to the escalation playbook.
- **Regression tests**: persona changes must not weaken safety.

