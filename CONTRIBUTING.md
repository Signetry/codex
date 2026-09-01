# Contributing to Signetry for Codex

Contributions are welcome — bug reports, docs fixes, and improvements to the Codex
integration surface (the MCP server wiring, the lifecycle-hook guard, `config.toml`).

## Licensing

This repository is **[Apache-2.0](LICENSE)**. You may use, copy, modify, distribute,
and commercialize it, including in closed-source and commercial products, subject to
the licence's attribution and notice terms. There is no separate permission to ask for.

It is part of Signetry's
[open-core model](https://github.com/Signetry/signetry/blob/main/LICENSING.md): the
integration surface (this repo and the other adapters) is Apache-2.0 so anyone can add
an agent, an editor, or a CI adapter, while the engine
([`Signetry/core`](https://github.com/Signetry/core)) is source-available under
BUSL-1.1 and converts to Apache-2.0 on 2030-08-31.

### The CLA still applies

Contributions are accepted under the [Contributor License Agreement](CLA.md), and the
CLA check still gates every pull request. Open core is exactly why it is kept: a
well-built adapter here may later be moved *across* the open-core line into the
BUSL-1.1 engine, or re-licensed as the model evolves. The CLA is what makes that
possible without going back to every past contributor for permission. It does not take
your Apache-2.0 rights away — you keep the same rights to this code as everyone else.

## Signing the CLA (required before merge)

This is enforced by a bot. When you open a pull request, the **CLA Assistant** check
will ask you to sign the [Contributor License Agreement](CLA.md). Reply on the PR
with exactly:

```
I have read the CLA Document and I hereby sign the CLA
```

Your acceptance is recorded in `signatures/cla.json`. A PR **cannot be merged** until
the CLA is signed.

## Getting started

This repo has no build step and no test suite: it is documentation plus the
`config.toml` snippet for `~/.codex/config.toml`. The governance logic lives in
`signetry-core`, which this integration pins and never reimplements.

To exercise a change locally, install the pinned engine (same command as the README
prerequisite):

```bash
pip install "signetry-core @ git+https://github.com/Signetry/core@v0.7.0"
```

Then point Codex at the MCP server / guard as described in the [README](README.md) and
confirm the behaviour you changed.

Two workflows run on your pull request:

- **CLA** (`.github/workflows/cla.yml`) — the signature gate described above.
- **Reviewer** (`.github/workflows/reviewer.yml`) — an advisory architecture/security
  review that posts one comment. It is advisory only: it never merges and never fails
  the PR.

When you change a pinned version, keep it consistent across `README.md`,
`config.toml`, and the workflows, and note it under `## [Unreleased]` in
[CHANGELOG.md](CHANGELOG.md).

## Credit

Contributors are **acknowledged** in [CONTRIBUTORS.md](CONTRIBUTORS.md), the Git
history, and release notes. See the "Recognition of Contributors" clause in
[CLA.md](CLA.md).
