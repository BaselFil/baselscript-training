# BaselScript AI Reference

This directory is the single current reference root used for AI-assisted BaselScript generation,
review and validation.

## Entry points

```text
reference/manifest.json
reference/AI_CONTEXT.md
```

Do not treat the repository corpus as direct generation authority. The manifest routes only the
current machine contract, semantics and composition patterns needed for a task.

## Layers

```text
reference/language/     machine-readable contract
reference/semantics/    verified source semantics
reference/patterns/     complete composition/lifecycle patterns
reference/knowledge/    AI generation policy
reference/evidence/     audits and evidence
reference/regression/   confirmed/negative/open runtime tests
reference/examples/     curated examples
```

## Loading model

```text
manifest.json
    -> baseline_required
    -> all matching task_routes
    -> route_expansion_rules
    -> generate/review
```

Do not load the full reference by default.

## Maintenance rule

A new authoritative rule must have one home. Update the routed semantic/policy file and then update
`manifest.json` if routing changes. Do not create parallel `old`, `new`, `ui_layout`, or duplicate
authoritative files for the same rule.
