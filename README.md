# stacklit-action

Keep your [Stacklit](https://github.com/glincker/stacklit) codebase index (`stacklit.json` + `DEPENDENCIES.md`) up to date automatically via GitHub Actions.

## Modes

| Mode | Behavior |
|------|----------|
| `auto-commit` (default) | Regenerates the index and commits any changes back to the branch |
| `check` | Fails the workflow if the index is stale  - useful for PRs |

## Usage

### Auto-commit on push

Regenerate and commit the index whenever code changes land on `main`:

```yaml
name: Update stacklit index
on:
  push:
    branches: [main]
    paths-ignore: ['stacklit.json', 'DEPENDENCIES.md', '**.md']

jobs:
  stacklit:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v4
      - uses: glincker/stacklit-action@v1
```

### Check mode (PR gate)

Fail the PR if the index wasn't updated:

```yaml
name: Check stacklit index
on: [pull_request]

jobs:
  stacklit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: glincker/stacklit-action@v1
        with:
          mode: check
```

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `mode` | `auto-commit` | `auto-commit` or `check` |
| `version` | `latest` | Stacklit version (e.g. `v0.3.0`) |
| `args` | `""` | Extra args for `stacklit generate` |

## Permissions

Auto-commit mode requires `permissions: contents: write` on the job.

## License

MIT
