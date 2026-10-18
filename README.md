# React behavior pack

This repository supplies React guidance to the [agents workflow](https://github.com/Dolan001/agents).
It contains no runnable frontend. The workflow uses it to write and verify the target project's
`apps/frontend/` application.

## When to use it

Declare `Frontend framework: React` in the PRD. The `reactjs` pack is selected automatically,
alongside the chosen DRF or FastAPI backend pack. `$start-build` runs the full workflow; to stop at
the frontend gate, use:

```bash
./.agents/bin/ai start-frontend --project . --github-user <your-github-user>
```

The PRD or saved state supplies React, so `--frontend react` is optional when already declared.
The frontend consumes a verified backend and approved HTML baseline; it does not silently repair
missing prerequisites. No command pushes by default.

## What it creates and verifies

Generated code uses strict TypeScript, accessible semantic components, and explicit loading,
empty, success, validation, unauthorized, forbidden, timeout, error, and retry states. The network
boundary uses a generated typed client with runtime response validation. Work is scoped by PRD
requirement IDs and path leases, not a broad application rewrite.

The independent gate checks structure, frontend tests, accessibility and responsive states,
approved-HTML design fidelity, and live backend integration. Fixtures can support early tests but
cannot replace final integration evidence. Results and checkpoints live under `.ai/` in the target;
inspect them with `$workflow-status`.

## Pack contents

`agents/` defines roles; `skills/` covers inspection, scaffolding, feature slices, realtime work,
and verification; `rules/`, `hooks/`, and `commands/` constrain changes. This guidance is never
copied into the generated application.
