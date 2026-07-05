# 🛡️ Go Guardian

**All-in-one Go quality gate for GitHub Actions.** Run `gofmt`, `go vet`, `golangci-lint`, and `go test` in a single step — with PR annotations and configurable failure severity.

![Go Guardian](https://img.shields.io/badge/go-%2300ADD8.svg?style=for-the-badge&logo=go&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/github%20actions-%232671E5.svg?style=for-the-badge&logo=githubactions&logoColor=white)
![MIT License](https://img.shields.io/badge/license-MIT-green?style=for-the-badge)

---

## 🔧 Quick Start

```yaml
name: Go Quality Gate

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-go@v5
        with:
          go-version: '1.22'

      # One step. Four checks. Zero excuses.
      - uses: CodeGero/go-guardian@v1
        with:
          working-directory: '.'
          severity: error
```

---

## 📋 Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `go-version` | No | _from go.mod_ | Go version to use. If empty, reads from go.mod. |
| `working-directory` | No | `.` | Directory containing `go.mod`. |
| `skip-lint` | No | `false` | Skip `golangci-lint`. |
| `skip-vet` | No | `false` | Skip `go vet`. |
| `skip-fmt` | No | `false` | Skip `gofmt`. |
| `skip-test` | No | `false` | Skip `go test`. |
| `lint-args` | No | `--timeout=5m` | Extra args for `golangci-lint run`. |
| `test-args` | No | `-v -race -count=1 ./...` | Extra args for `go test`. |
| `severity` | No | `error` | `error` → fails job; `warning` → annotates but passes. |
| `annotate` | No | `true` | Post GitHub PR annotations per finding (needs token). |

## 📤 Outputs

| Output | Description |
|--------|-------------|
| `go-version` | Go version used during the run. |
| `lint-report` | Path to the golangci-lint report file. |
| `test-report` | Path to the go test JSON output. |

---

## 🎯 What It Checks

| Check | Tool | What It Catches |
|-------|------|-----------------|
| **Format** | `gofmt -l` | Ungroomed code — inconsistent spacing, braces, indentation. |
| **Static analysis** | `go vet` | Suspicious constructs — unreachable code, bad printf verbs, unsafe pointer misuse. |
| **Full lint** | `golangci-lint` | Dead code, bugs, complexity, style violations — 50+ linters in one pass. |
| **Tests** | `go test` | Run your test suite with race detection enabled. |

---

## ⚙️ Advanced Usage

### Skip specific checks

```yaml
- uses: CodeGero/go-guardian@v1
  with:
    skip-fmt: true         # gofmt already handled by IDE
    skip-lint: true        # running lint separately
    severity: warning
```

### Custom test flags

```yaml
- uses: CodeGero/go-guardian@v1
  with:
    test-args: '-v -race -count=1 -coverprofile=coverage.out -covermode=atomic ./...'
    lint-args: '--timeout=10m --config=.golangci.yml'
```

### Warning-only mode (don't block the pipeline)

```yaml
- uses: CodeGero/go-guardian@v1
  with:
    severity: warning
```

---

## ⭐ Upgrading to Premium

The free Go Guardian covers the essential quality checks. For teams shipping to production, **[Go Guardian Premium](https://kryptorious.gumroad.com/l/jbvet)** ($9 lifetime) unlocks:

- 🔒 **Parallel execution** — run all 4 checks concurrently, cut CI time by >60%
- 📊 **Historical trend dashboard** — see lint/test trends over time
- 🧵 **Per-PR summary comment** — bot posts a formatted summary directly on the pull request
- 🎯 **Severity thresholds** — fail on new issues only, not pre-existing ones (baseline mode)
- 📈 **Code coverage gate** — enforce minimum coverage % before merge
- 🔐 **Private repository support** with zero configuration

👉 **[Get Premium — $9 lifetime](https://kryptorious.gumroad.com/l/jbvet)**

---

## 📦 Repository

- **GitHub:** [CodeGero/go-guardian](https://github.com/CodeGero/go-guardian)
- **Author:** [@kryptorious](https://github.com/kryptorious)
- **License:** MIT

---

## 🏗️ Built by CodeGero

More quality gate actions from CodeGero:

| Action | Category |
|--------|----------|
| [**rust-guardian**](https://github.com/CodeGero/rust-guardian) | All-in-one Rust quality gate |
| [**license-guardian**](https://github.com/CodeGero/license-guardian) | Dependency license compliance |

---

<p align="center">
  <sub>Made with ❤️ by <a href="https://github.com/kryptorious">kryptorious</a> for the <a href="https://github.com/CodeGero">CodeGero</a> org</sub>
</p>
