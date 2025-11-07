# gloo.foo

> **Shell-style programming framework for Go**

[![Go Version](https://img.shields.io/badge/Go-1.25+-00ADD8?style=flat&logo=go)](https://golang.org/)
[![License](https://img.shields.io/badge/license-AGPL--3.0-blue.svg)](LICENSE)
[![GitHub Stars](https://img.shields.io/github/stars/gloo-foo/framework?style=social)](https://github.com/gloo-foo/framework)

## What is gloo.foo?

gloo.foo is a **Go framework for building composable, shell-style commands**. Create commands that pipe together like Unix utilities, but with Go's type safety, performance, and zero-dependency portability.

```go
// Build pipelines with composable commands
pipeline := gloo.Pipe(
    mycommands.Read("input.txt"),
    mycommands.Filter(criteria),
    mycommands.Transform(options),
    mycommands.Write("output.txt"),
)

err := gloo.Run(pipeline)
```

## Why gloo.foo?

**Shell composability meets Go reliability:**

- 🐚 **Familiar patterns** - Commands pipe together naturally
- 🔥 **Type safety** - Catch errors at compile time
- ⚡ **Performance** - In-memory operations, no subprocess overhead
- 📦 **Zero dependencies** - Single binary, works anywhere
- 🧩 **Modular** - Each command is an independent Go module
- 🌍 **Extensible** - Third-party commands work seamlessly

## Quick Start

```bash
go get github.com/gloo-foo/framework
```

```go
package mycommand

import gloo "github.com/gloo-foo/framework"

type flags struct {
    IgnoreCase bool
}

type command gloo.Inputs[gloo.File, flags]

func New(pattern string, parameters ...any) gloo.Command {
    return command(gloo.Initialize[gloo.File, flags](append(parameters, pattern)...))
}

func (c command) Executor() gloo.CommandExecutor {
    return gloo.Inputs[gloo.File, flags](c).Wrap(
        gloo.LineTransform(func(line string) (string, bool) {
            // Your processing logic here
            return line, true
        }).Executor(),
    )
}

func IgnoreCase(f *flags) { f.IgnoreCase = true }
```

## Learn More

- **[📚 Framework Documentation](https://github.com/gloo-foo/framework/wiki)** - Complete guide with patterns and examples
- **[🚀 yup.sh Commands](https://github.com/yupsh)** - Production-ready command implementations
- **[💬 Discussions](https://github.com/gloo-foo/framework/discussions)** - Ask questions and share ideas
- **[🤝 Contributing](https://github.com/gloo-foo/framework/blob/main/.github/CONTRIBUTING.md)** - Join the community

---

<p align="center">
  <strong>Built for developers who want shell power with Go reliability</strong>
</p>
<p align="center">
  <a href="https://github.com/gloo-foo/framework">Framework</a> •
  <a href="https://github.com/gloo-foo/framework/wiki">Documentation</a> •
  <a href="https://github.com/yupsh">Examples</a> •
  <a href="https://github.com/gloo-foo/framework/discussions">Discussions</a>
</p>
