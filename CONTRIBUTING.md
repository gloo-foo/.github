# Contributing to gloo.foo

Thanks for your interest! Contributions are welcome — reporting bugs, improving docs, adding flags to existing commands, or building new ones.

## Development Process

We use GitHub to host code, track issues, and review changes. To propose a change:

1. Fork the repo and branch from `main`.
2. Make your change, with tests that prove the intended behavior.
3. Ensure the quality gate passes: `make check`.
4. Open a pull request describing what changed and why.

## The Quality Gate

Every module ships with a self-documenting `Makefile`. A change is complete only when `make check` exits zero. It runs:

- `gofumpt` formatting, `go vet`, `golangci-lint`, and `staticcheck` — zero findings
- `gocognit` — cognitive complexity ≤ 7 for every production function
- `govulncheck` — no known vulnerabilities
- **100% statement coverage** — tests must cover every line, and must prove _intended_ behavior (a bug that a test locks in is two bugs, not coverage)
- `goreleaser check` — valid release config

Tooling is pinned in each module's `go.mod` `tool` stanza and run via `go tool` — no global installs.

## Anatomy of a Command

gloo.foo uses a **modular architecture**: each command is its own independent Go module at `github.com/gloo-foo/cmd-<name>`, built on the framework's composition patterns. Your command is a first-class citizen — it composes exactly like the built-ins.

A command module looks like this:

```
cmd-<name>/
├── go.mod              # module github.com/gloo-foo/cmd-<name>
├── command.go          # the constructor: <Name>(opts ...any) gloo.Command[[]byte, []byte]
├── opt.go              # sealed flag types implementing Switch[flags]
├── command_test.go     # unit tests (via the testable harness)
├── alias/              # unprefixed re-exports for ergonomic import sites
├── examples/           # godoc Example tests, one per flag
└── README.md           # CI + godoc badges
```

The constructor parses arguments with `gloo.NewParameters[gloo.File, flags]` and returns a single framework pattern (`patterns.Map`, `Filter`, `Accumulate`, `Aggregate`, `Expand`, or their stateful variants) — you supply only the per-line algorithm; the pattern handles all stream wiring. For the few cases no pattern covers, use `FuncCommand` directly.

The clearest way to learn the shape is to read a small, complete example:

- **Canonical exemplar**: [cmd-cat](https://github.com/gloo-foo/cmd-cat) — constructor, sealed flags, alias re-exports, and intent-proving tests.
- **Pattern guide**: the [For Command Authors](https://github.com/gloo-foo/framework/wiki/For-Command-Authors) wiki page — which pattern to pick and why.

## Conventions

- **Filesystem**: never touch the OS filesystem directly — accept an injected `afero.Fs`. Tests use `afero.NewMemMapFs()`.
- **Errors**: define constant sentinel errors (`type Error string`) so every path is matchable with `errors.Is`; reserve `fmt.Errorf` for wrapping with `%w`.
- **Receivers**: prefer immutable value types; use pointer receivers only for genuine per-run state.
- **Flags**: strongly typed and sealed — see `opt.go` in any command.
- Follow standard Go style (`gofumpt`); keep functions small and focused.

## Reporting Bugs

We use GitHub issues. Open one with the [bug report template](https://github.com/gloo-foo/.github/blob/master/ISSUE_TEMPLATE/bug_report.yml). Great reports include a summary, steps to reproduce, expected vs. actual behavior, and your Go/OS versions.

## License

By contributing, you agree that your contributions will be licensed under the project's **MIT** license.

## References

- [Framework](https://github.com/gloo-foo/framework) · [Documentation (wiki)](https://github.com/gloo-foo/framework/wiki)
- [Commands](https://github.com/orgs/gloo-foo/repositories?q=cmd-) · [Examples](https://github.com/gloo-foo/examples)
- [Code of Conduct](https://github.com/gloo-foo/.github/blob/master/CODE_OF_CONDUCT.md)
