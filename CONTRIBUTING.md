# Contributing

## Scope

This repo is **documentation-first**. The goal is to keep the concepts, guides, and examples aligned and internally consistent.

## Style

- **Be explicit**: define assumptions, inputs, outputs, and failure modes.
- **Prefer composable primitives**: persona, context, policies, escalation.
- **Link aggressively**: when you introduce a term, link to `glossary.md` or the relevant concept doc.
- **Use runnable thinking**: even in docs, specify interfaces (schemas, steps, states).

## Repo conventions

- Concepts live in `concepts/`
- How-to guides live in `guides/`

## Adding a concept or guide

1. Put concepts under `concepts/`, how-to guides under `guides/`.
2. Be explicit: define assumptions, inputs, outputs, and failure modes.
3. Cross-link related terms to `glossary.md` and the relevant concept docs
   (e.g. `concepts/personas.md`, `concepts/safety-model.md`).

