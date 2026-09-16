# Generated React structure

The agent generates this structure under target `apps/frontend/`. Router and build
tool choices are recorded from project constraints; route/feature names come from the
requirements.

```text
apps/frontend/
├── package.json
├── package-lock.json
├── tsconfig.json
├── vite.config.ts
├── eslint.config.js
├── playwright.config.ts
├── vitest.config.ts
├── index.html
├── .env.example
├── .gitignore
├── .dockerignore
├── Dockerfile
├── README.md
├── public/
├── src/
│   ├── main.tsx
│   ├── app/
│   │   ├── App.tsx
│   │   ├── router.tsx
│   │   ├── providers.tsx
│   │   └── styles.css
│   ├── routes/
│   │   ├── RootRoute.tsx
│   │   ├── NotFoundRoute.tsx
│   │   └── <FeatureRoute>.tsx
│   ├── features/
│   │   └── <feature>/
│   │       ├── components/
│   │       ├── hooks/
│   │       ├── schemas.ts
│   │       ├── types.ts
│   │       └── tests/
│   ├── components/
│   │   ├── ui/
│   │   └── shared/
│   ├── services/
│   │   ├── api-client.ts
│   │   └── errors.ts
│   └── lib/
│       ├── env.ts
│       └── utilities.ts
├── tests/
│   ├── setup.ts
│   └── e2e/
└── scripts/
    └── validate_project.mjs
```

Ownership:

- `src/app`: application assembly, providers, router, and global styles.
- `src/routes`: route-level composition only.
- `src/features`: cohesive product behavior with colocated validation and tests.
- `src/components/ui`: reusable presentation primitives without business behavior.
- `src/services`: typed network boundaries and standard client errors.
- `src/lib`: validated environment access and framework-neutral utilities.
- shared API types/client are generated under target `packages/` when required.
- Realtime features activate one typed `src/realtime` client plus unit/browser tests. It owns
  connection lifecycle, backoff, cursor resync, gap detection, deduplication, bounded queues, and
  degraded state; feature hooks consume it rather than opening component sockets.

Naming and growth rules:

- Create a feature map from PRD journeys, screens, entities, and API resources before files. Keep
  exact, understandable PRD product terms. If a capability is unnamed, use familiar names such as
  `authentications`, `accounts`, `notifications`, `articles`, `tasks`, `webhooks`, or `chat`.
- Do not invent vague features or routes such as `identity`, `work`, `operations`,
  `data-management`, `management`, or `collaboration` unless the PRD intentionally uses that term.
- Use kebab-case feature directories and responsibility-based PascalCase components: for example,
  `features/authentications/components/SignInForm.tsx` and `routes/TasksRoute.tsx`. Avoid
  `DataComponent`, `MainView`, `Helper`, catch-all `index.tsx` implementations, and numbered names.
- Route files compose feature screens; they do not contain a whole feature. Split components,
  hooks, API adapters, schemas, state, and tests by behavior. Keep route/feature source files at
  most 250 lines and preserve accessible states and focused tests during every split.

Generation order:

1. Resolve supported Node, React, TypeScript, router, and build-tool versions.
2. Create strict configuration, environment validation, app assembly, routing,
   dependency lock, and test foundations. When Docker development is selected,
   create or reuse the development Compose runtime before installing dependencies
   or executing checks; root `compose.frontend.yaml` belongs to the frontend lease.
3. Convert approved design evidence into semantic route/feature boundaries.
4. Produce the PRD-to-feature map, then generate only requirement-backed features.
5. Connect the generated typed API client and runtime-validate responses.
6. Add unit, integration, accessibility, responsive, and visual tests.
7. Add production Docker packaging and CI after the production build passes.
   The development Docker runtime from step 2 is already available for these checks.
