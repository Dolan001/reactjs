---
name: create-react-monorepo-frontend
description: Create the React frontend structure in a new target monorepo from approved design and architecture contracts.
---

# Create React frontend

Generate only declared structure paths. Resolve supported versions, enable strict
TypeScript, establish routing, error boundaries, styling, runtime API validation,
testing, environment validation, and production build checks. Keep global state
minimal and create only shared primitives required by planned slices.
First emit the PRD-to-feature/route map. Use explicit product nouns or justified familiar
capability names, then scaffold responsibility-named components, hooks, API adapters, and tests.

Read `../../rules/project-structure.md` and
`../implement-react-vertical-slice/references/production-delivery.md` before selecting
routing, state, data, accessibility, security, or test boundaries.

Honor the project's development-runtime choice. For Docker development or an npm
installation failure, read `references/dependency-runtime.md` before installing.
Create the development runtime before running build checks; production packaging
and CI follow a passing build. Record executable commands in the workflow's grouped
test-command manifest and preserve the distinction between implementation checks
and independent approval.
