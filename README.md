# Signetry for Codex

> **Copyright (c) 2026 Binay Dalai. All rights reserved.**
> This repository is strictly for viewing and contributing to the original project. You may not use, copy, modify, distribute, or commercialize this code for your own personal or commercial projects without explicit written permission. Only the original author retains the right to use and monetize this project.


Govern coding-agent changes in [OpenAI Codex](https://developers.openai.com/codex)
with [signetry-core](https://github.com/Signetry/core).

## Prerequisite

```bash
pip install "signetry-core @ git+https://github.com/Signetry/core@v0.7.0"
```

Add a `.signetry/admission.yaml` to your repo (allowed/forbidden paths, diff budget,
required checks). A conservative default applies without one.

## 1. MCP server (recommended)

Codex reads MCP servers from `~/.codex/config.toml`. Add Signetry's server so the
agent can run admission / verify / provenance itself:

```toml
[mcp_servers.signetry]
command = "python"
args = ["-m", "signetry_core.mcp_server"]

[mcp_servers.signetry.env]
SIGNETRY_MCP_ROOTS = "/absolute/path/to/your/repo"
```

`SIGNETRY_MCP_ROOTS` scopes the server to your workspace(s) so it can't be pointed
at arbitrary host paths. The agent then has `signetry_admit`, `signetry_verify`, and
`signetry_provenance` tools.

## 2. Lifecycle hook guard (deterministic pre-action check)

Codex supports lifecycle hooks. Configure a hook that runs `signetry guard` before a
file write / command, so an out-of-scope or forbidden action is blocked by
deterministic code (not the model). See the Codex config docs for the exact hook
schema for your version; the guard command to wire in is:

```bash
signetry guard --repo "$REPO" --path "$PROPOSED_PATH"      # exit 1 = deny
signetry guard --repo "$REPO" --command "$PROPOSED_COMMAND" # exit 1 = deny
```

`signetry guard` exits non-zero and prints a reason when the action violates the
contract; exit 0 means allowed.

## 3. The durable guarantee: CI

Whichever agent opens the PR, make **Signetry Admission** a required check so nothing
merges without a signed receipt:
<https://github.com/marketplace/actions/signetry-admission>. In-editor guards are
best-effort defense-in-depth; the CI check is the enforced gate.

## 4. Scan & fix

`signetry-core` also finds vulnerabilities and can govern the fix with Codex:

```bash
signetry scan .                              # SAST over the repo (7 languages, SARIF), offline & free
signetry scan . --fix --fix-agent codex-cli  # draft a governed fix → admission → signed receipt
```

`--fix` is **bring-your-own-key** (your `OPENAI_API_KEY`, never shared, redacted
from every artifact) and opens **branch-only** PRs — never merges. Works with
OpenAI-compatible gateways (e.g. IBM ICA) via `--codex_model` / base-URL inputs.
See [signetry-core: AUTOFIX_SETUP.md](https://github.com/Signetry/core/blob/main/docs/AUTOFIX_SETUP.md).

---

Part of the [Signetry platform](https://github.com/Signetry/signetry). Governance logic lives in [signetry-core](https://github.com/Signetry/core); this integration never reimplements policy and never auto-merges.

## License

**Copyright (c) 2026 Binay Dalai. All rights reserved.** This code is not open source. You may not use, copy, modify, distribute, or commercialize it for your own personal or commercial purposes without explicit written permission from the author, who alone retains the right to use and monetize this project. See [CONTRIBUTING.md](CONTRIBUTING.md).
