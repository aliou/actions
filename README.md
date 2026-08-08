# actions

Reusable GitHub Actions and workflows, shared across my repos.

## Usage

Reference an action from a subdirectory:

```yaml
- uses: aliou/actions/<action-name>@v1
```

Reference a reusable workflow:

```yaml
jobs:
  call:
    uses: aliou/actions/.github/workflows/<workflow>.yml@v1
```

Pin to a tag (`v1`), a specific SHA, or a branch as needed.
