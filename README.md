# LucaTools Actions

GitHub Actions for installing and using [Luca](https://luca.tools) — the lightweight CLI tool manager.

---

## Actions

### `install-luca`

Installs the Luca CLI on the runner.

```yaml
- uses: LucaTools/actions/install-luca@v1
```

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `version` | Luca version to install (e.g. `"1.2.3"`). Omit to use the latest release. | No | `""` (latest) |

---

### `install-tools`

Installs the tools defined in a Luca spec file and adds them to `PATH` for subsequent steps.

> **Requires** `install-luca` to have run first.

```yaml
- uses: LucaTools/actions/install-tools@v1
```

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `spec` | Path to the spec file, relative to the workspace root. | No | `Lucafile` |

After this action runs, all tools listed in the spec file are available by name in subsequent steps (e.g. `swiftlint --version`).

---

## Usage

### Minimal setup

```yaml
steps:
  - uses: actions/checkout@v4

  - uses: LucaTools/actions/install-luca@v1

  - uses: LucaTools/actions/install-tools@v1

  - run: swiftlint --version
```

### Pin a specific Luca version

```yaml
- uses: LucaTools/actions/install-luca@v1
  with:
    version: "1.2.3"
```

### Use a custom spec file name

```yaml
- uses: LucaTools/actions/install-tools@v1
  with:
    spec: Lucafile-ci
```

### Full workflow example

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: macos-latest

    steps:
      - uses: actions/checkout@v4

      - uses: LucaTools/actions/install-luca@v1

      - uses: LucaTools/actions/install-tools@v1

      - run: swiftlint --version
      - run: tuist version
```

---

## Versioning

Actions follow [semantic versioning](https://semver.org). A floating major tag (`v1`, `v2`, …) is always updated to point to the latest release within that major version. Pinning to a major tag (e.g. `@v1`) is recommended for most workflows — you will receive non-breaking updates automatically.

To pin to an exact release use the full tag (e.g. `@v1.2.3`).

---

## License

[Apache 2.0](LICENSE)
