# Umbra for Codex

> **Copyright (c) 2026 Binay Dalai. All rights reserved.**
> This repository is strictly for viewing and contributing to the original project. You may not use, copy, modify, distribute, or commercialize this code for your own personal or commercial projects without explicit written permission. Only the original author retains the right to use and monetize this project.


Govern coding-agent changes in [OpenAI Codex](https://developers.openai.com/codex)
with [umbra-core](https://github.com/bkd-dotcom/umbra-core).

## Prerequisite

```bash
pip install "umbra-core @ git+https://github.com/bkd-dotcom/umbra-core@v0.5.4"
```

Add a `.umbra/admission.yaml` to your repo (allowed/forbidden paths, diff budget,
required checks). A conservative default applies without one.

## 1. MCP server (recommended)

Codex reads MCP servers from `~/.codex/config.toml`. Add Umbra's server so the
agent can run admission / verify / provenance itself:

```toml
[mcp_servers.umbra]
command = "python"
args = ["-m", "umbra_core.mcp_server"]

[mcp_servers.umbra.env]
UMBRA_MCP_ROOTS = "/absolute/path/to/your/repo"
```

`UMBRA_MCP_ROOTS` scopes the server to your workspace(s) so it can't be pointed
at arbitrary host paths. The agent then has `umbra_admit`, `umbra_verify`, and
`umbra_provenance` tools.

## 2. Lifecycle hook guard (deterministic pre-action check)

Codex supports lifecycle hooks. Configure a hook that runs `umbra guard` before a
file write / command, so an out-of-scope or forbidden action is blocked by
deterministic code (not the model). See the Codex config docs for the exact hook
schema for your version; the guard command to wire in is:

```bash
umbra guard --repo "$REPO" --path "$PROPOSED_PATH"      # exit 1 = deny
umbra guard --repo "$REPO" --command "$PROPOSED_COMMAND" # exit 1 = deny
```

`umbra guard` exits non-zero and prints a reason when the action violates the
contract; exit 0 means allowed.

## 3. The durable guarantee: CI

Whichever agent opens the PR, make **Umbra Admission** a required check so nothing
merges without a signed receipt:
<https://github.com/marketplace/actions/umbra-admission>. In-editor guards are
best-effort defense-in-depth; the CI check is the enforced gate.

## 4. Scan & fix

`umbra-core` also finds vulnerabilities and can govern the fix with Codex:

```bash
umbra scan .                              # SAST over the repo (7 languages, SARIF), offline & free
umbra scan . --fix --fix-agent codex-cli  # draft a governed fix → admission → signed receipt
```

`--fix` is **bring-your-own-key** (your `OPENAI_API_KEY`, never shared, redacted
from every artifact) and opens **branch-only** PRs — never merges. Works with
OpenAI-compatible gateways (e.g. IBM ICA) via `--codex_model` / base-URL inputs.
See [umbra-core: AUTOFIX_SETUP.md](https://github.com/bkd-dotcom/umbra-core/blob/main/docs/AUTOFIX_SETUP.md).

---

Part of the [Umbra platform](https://github.com/bkd-dotcom/umbra-umbrella). Governance logic lives in [umbra-core](https://github.com/bkd-dotcom/umbra-core); this integration never reimplements policy and never auto-merges.

## License

**Copyright (c) 2026 Binay Dalai. All rights reserved.** This code is not open source. You may not use, copy, modify, distribute, or commercialize it for your own personal or commercial purposes without explicit written permission from the author, who alone retains the right to use and monetize this project. See [CONTRIBUTING.md](CONTRIBUTING.md).
