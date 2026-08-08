# actions

Reusable GitHub Actions and workflows, shared across my repos.

Pinned to `@main` for now — no version tags yet, still iterating. Not yet
consumed by any repo; consumers are being migrated once dependency/version
drift across repos (pnpm-action-setup, setup-node, node versions, etc.) is
reconciled.

## Composite actions

### `pnpm-install`

Installs pnpm, Node.js, and dependencies with caching.

```yaml
- uses: aliou/actions/pnpm-install@main
  with:
    node-version: "24" # optional, defaults to "24"
```

### `tailscale-oidc`

Connects to Tailscale using a GitHub OIDC token exchanged for a short-lived
Tailscale API token — no long-lived secret needed. The calling job must
grant `permissions: { id-token: write }`.

```yaml
permissions:
  id-token: write

steps:
  - uses: aliou/actions/tailscale-oidc@main
    with:
      oauth-client-id: ${{ secrets.TS_OAUTH_CLIENT_ID }}
      audience: ${{ secrets.TS_AUDIENCE }}
      tags: tag:ci-evals # optional, defaults to "tag:ci-evals"
```

## Reusable workflows

### `publish.yml`

Changesets-driven release flow: opens/updates a release PR, or stages and
publishes packages (via npm's OIDC trusted publishing, no `NPM_TOKEN`
needed) and cuts a GitHub release when there's nothing left to release.
The calling job must grant contents/packages/pull-requests/id-token write
permissions.

```yaml
jobs:
  publish:
    permissions:
      contents: write
      packages: write
      pull-requests: write
      id-token: write
    uses: aliou/actions/.github/workflows/publish.yml@main
    with:
      node-version: "24" # optional, defaults to "24"
      npm-scope: "@aliou" # optional, defaults to "@aliou"
```

### `check-pi-install.yml`

Smoke-tests that a Pi extension package installs the way a user would
install it: `pi install https://github.com/<repository>@<ref>`.

```yaml
jobs:
  check-pi-install:
    uses: aliou/actions/.github/workflows/check-pi-install.yml@main
    with:
      repository: aliou/pi-linkup
      ref: ${{ github.event.pull_request.head.sha || github.sha }}
```
