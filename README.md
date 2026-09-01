# GITHUB WORKFLOW FOR Hodfords Opensource

This is a github workflow for Hodfords Opensource. It will run the test and build the project.

## Package manager

`lint.yaml` and `publish.yaml` accept a `package_manager` input: either `npm` (default) or `pnpm`.
Repositories that have not migrated keep working without any change — omitting the input keeps the
existing npm behaviour.

Repositories on pnpm must declare the pinned version in `package.json`, because
`pnpm/action-setup` resolves it from there:

```json
{
    "packageManager": "pnpm@12.0.0"
}
```

With `package_manager: pnpm` the workflows run `pnpm install --frozen-lockfile`, so `pnpm-lock.yaml`
has to be committed and in sync with `package.json`.

### lint.yaml

| Input             | Required | Default | Description                       |
| ----------------- | -------- | ------- | --------------------------------- |
| `package_manager` | no       | `npm`   | Package manager: `npm` or `pnpm`. |

```yaml
jobs:
    lint:
        uses: hodfords-solutions/actions/.github/workflows/lint.yaml@main
        with:
            package_manager: pnpm
```

### publish.yaml

| Input             | Required | Default     | Description                                                     |
| ----------------- | -------- | ----------- | --------------------------------------------------------------- |
| `build_path`      | no       | `dist/libs` | Directory (or `;`-separated directories) to publish from.         |
| `package_path`    | no       | `./`        | Directory that contains `package.json`.                           |
| `package_manager` | no       | `npm`       | Package manager: `npm` or `pnpm`.                                 |

```yaml
jobs:
    build:
        uses: hodfords-solutions/actions/.github/workflows/publish.yaml@main
        with:
            build_path: dist/lib
            package_manager: pnpm
        secrets:
            NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```

### update-doc.yaml

Package-manager agnostic — it only reads `package.json` and `README.md`, so it needs no input.
