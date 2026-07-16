# Building with Syni

A practical flow for building a Syni-powered experience.

> Before you start: Syni assumes HSI is produced upstream by
> the Synheart engine (via the `synheart-core-*` SDKs). If your app does
> not already have HSI input, integrate `synheart-core-*` first.

## Step 1: Choose where the LLM runs

- On-device (`syni-runtime`), cloud (Syni Cloud), or hybrid: see `concepts/on-device-vs-cloud.md`.
- This is a decision about *the LLM*, not about state inference. HSI is always produced on-device.

## Step 2: Define your persona

Start with:

- role + audience
- voice
- goals
- boundaries
- escalation triggers

Author the persona as a spec in `syni-spec/personas/research/` first, then promote to `prod/` once it passes conformance.

See `concepts/personas.md` and `guides/creating-personas.md`.

## Step 3: Decide which HSI axes you care about

Pick the subset of HSI your experience actually needs (e.g. `affective` + `cognitive` for a focus coach; add `physiological` for recovery work). Then:

- confirm your app's consent profile covers those axes,
- decide which axes get summarized into reduced cloud payloads vs. stay local-only,
- write expectations for low-confidence readings.

See `concepts/hsi.md`. Do not bypass HSI to read raw signals — extend the contract instead.

## Step 4: Implement safety first

Before tuning "helpfulness", ensure:

- refusals match `syni-spec/safety/`
- escalation transitions work end-to-end
- consent is honored — recall that consent enforcement lives in Synheart Core and Syni Cloud's consent service, *not* in Syni

See `concepts/safety-model.md`.

## Step 5: Wire the persona engine pipeline

Use the reference flow from `concepts/persona-engine.md`:

- normalize input (user message + HSI payload + history)
- classify intent + risk
- assemble context
- apply CCO framing (`concepts/cco.md`)
- generate (local `syni-runtime` or Syni Cloud)
- validate output against `syni-runtime/schemas/`

## Step 6: Add UI affordances

Syni works best when the UI cooperates:

- quick actions ("start a 10-minute sprint")
- HSI transparency ("focus reading: medium confidence — why am I seeing this?")
- escalation affordances (resources, "call a friend", etc.)
- offline indicators when only `syni-runtime` is available

## Step 7: Conform and iterate

Run the relevant subset of `syni-spec/conformance/` against your SDK integration. Then iterate with research workflows — promote persona changes from `research/` → `prod/` only after style + boundary tests pass.
