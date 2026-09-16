# Frontend dependency runtime and recovery

Use this reference when the project selects Docker development or npm installation
fails. Preserve an explicitly selected native runtime; Docker is not mandatory for
every project, and this reference does not govern Flutter mobile toolchains.

## Docker development

- Reuse the project's frontend Compose service, or create a standalone root
  `compose.frontend.yaml` within the task lease. It must start frontend checks
  without backend secrets, database startup, or unfinished backend build contexts.
- Record a compatible Node/package-manager version and image digest. Mount the
  project at a stable container path so sibling API-client packages are accessible.
  Use project-scoped named volumes for Linux `node_modules` and npm cache; do not
  overwrite host dependencies or install packages globally on the host.
- Bind the development server to localhost on the host. Keep production settings,
  immutable application images and CI as subsequent work; do not wait for a passing
  production build to create the development runtime needed to run that build.
- Generate and retain the lockfile with normal peer validation. Confirm a clean
  `npm ci`, dependency tree, typecheck, lint, units, build and contract checks in
  the selected runtime. Browser tests that start their own server run in a separate
  container without competing for the development server's port.
- For Chromium-only headless tests, `playwright install --with-deps --only-shell
  chromium` avoids downloading a full browser unnecessarily. Install other browser
  targets when required by the test plan; a Chromium result does not verify them.

## Classify installation failures before retrying

- `ENOTFOUND` or registry timeout: check registry access from the actual container.
  Retry after connectivity or permission changes, not after changing package pins.
- npm `Cannot read properties of null (reading 'edgesOut')`: inspect the debug log's
  peer-resolution chain. A newer npm version is a diagnostic, not an assumed fix.
  In the observed Vite/Vitest case, npm selected a newer transitive Vite/devtools
  chain despite the root pin. A targeted package.json override
  `"overrides": { "vite": "$vite" }` kept it on the declared direct dependency and
  allowed npm 10.8.2 to install. Apply this only when the project directly depends
  on Vite and the log supports that diagnosis. Check peer compatibility and retain
  a lockfile; do not hard-code the incident's dependency versions in new projects.
- Do not use `--force`, `--legacy-peer-deps`, delete lockfiles, or upgrade global
  tools merely to suppress a resolver failure. Reproduce the repaired install and
  record the result before declaring success.
- `ENOSPC`: check both host space and the Docker filesystem; a free host disk does
  not imply free Docker disk space. Inspect `docker system df`. Remove only
  authorized disposable cache or temporary containers; preserve database volumes
  and unrelated images. Retry after space is available. Avoid copying large caches
  as a diagnostic. Remove task-owned temporary test containers when finished.

## Workflow evidence

Register argv arrays and host-relative cwd values in `.ai/test-commands.json` using
the grouped `commands` object, for example `frontend`, `contract`, `generate-client`
and `e2e`. The runner expects an object, not an array of named check records.
Use project-owned scripts or Docker Compose commands runnable from the repository
root. Preserve existing groups. A client-only phase must not fabricate successful
backend/integration commands to satisfy the later complete-matrix schema; record
those groups as deferred until their owning phases implement them.

Record exact commands, runtime, exit status and check scope. Preserve earlier
failure evidence, but remove resolved installation blockers from current evidence.
Keep independent verification pending until its actual gate runs. Successful
dependency recovery does not imply feature completion or production readiness.
