---
name: verify-react-delivery
description: Independently verify changed React slices or frontend delivery against requirements and the approved design baseline.
---

# Verify React delivery

Inspect the diff and run format, lint, types, affected units, production build, browser
journeys, accessibility, responsive, and localized visual checks. Verify runtime API
validation and required states. Capture evidence; do not edit source.
Use the project's declared runtime and registered command groups. For Docker
projects, verify inside the frontend containers; do not infer missing dependencies
from an absent host node_modules directory. Check the lockfile with a clean install
and use a separate container for browser tests that start their own server.
Independently validate feature/route names against the PRD/fallback policy, responsibility-based
filenames, compositional routes, cohesive ownership, and the 250-line split limit.

Reuse a passing shared full-matrix report only when its revision and workspace inputs
still match, then run focused changed-slice checks. Do not rerun the entire browser and
integration matrix per feature. For design fidelity, act independently from the
resolver and write the final `verification.json`; never let resolver comparison
evidence self-approve a repair.

Apply `../implement-react-vertical-slice/references/production-delivery.md` from a clean
verification context and compare against approved HTML/screenshots, not memory.
