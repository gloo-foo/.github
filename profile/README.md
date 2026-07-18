# gloo.foo

> **The Unix pipeline, reimagined as a first-class value.**

For half a century the pipe has been the most quietly radical idea in computing: small programs, each doing one thing well, joined end to end until simple parts add up to something powerful. gloo.foo asks a single question — what if that pipeline were not a line of shell text, but a real, typed value you could name, pass around, test, and compose like any other?

That is the whole of it. A source of data, a chain of transforms, and a sink that consumes the result, connected by a stream that behaves exactly like a shell pipe — backpressure, early teardown, and all — yet lives entirely inside one process. No subprocesses, no fragile string-slinging, no guessing whether two stages fit together. If they don't, you find out before anything runs, not after.

The organization gathers two things that belong together: a small framework that defines what a composable command actually _is_, and a growing library of familiar tools — the everyday verbs of the command line — rebuilt on top of it. Each tool is independent, each composes with every other, and anything you write yourself slots in beside them as an equal.

The result feels like the shell you already know and reads like the language you already write. Familiar on the surface, rigorous underneath.

## Explore

- **[gloo.foo](https://gloo.foo)** — the project home
- **[Framework](https://github.com/gloo-foo/framework)** — the core: types, patterns, and composition
- **[Commands](https://github.com/orgs/gloo-foo/repositories?q=cmd-)** — the everyday command line, rebuilt to compose
- **[Examples](https://github.com/gloo-foo/examples)** — runnable demonstrations

---

<p align="center"><em>Shell power. Type safety. One process.</em></p>
