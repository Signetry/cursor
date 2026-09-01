# Signetry for Cursor

[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](LICENSE)
[![PRs welcome (CLA)](https://img.shields.io/badge/PRs-welcome%20(CLA)-brightgreen.svg)](CONTRIBUTING.md)

Two ways to govern coding-agent changes in Cursor with
[signetry-core](https://github.com/Signetry/core).

## Prerequisite

```bash
pip install "signetry-core @ git+https://github.com/Signetry/core@v0.7.0"
```

Add a `.signetry/admission.yaml` to your repo declaring allowed/forbidden paths,
diff budget, and required checks (a conservative default applies without one).

## 1. MCP server (recommended)

Cursor speaks MCP. Add Signetry's server so the agent can run admission / verify /
provenance itself. Copy `mcp.json` to `.cursor/mcp.json` in your project (or merge
into your existing one):

```json
{
  "mcpServers": {
    "signetry": {
      "command": "python",
      "args": ["-m", "signetry_core.mcp_server"],
      "env": { "SIGNETRY_MCP_ROOTS": "${workspaceFolder}" }
    }
  }
}
```

Then the agent can call `signetry_admit`, `signetry_verify`, and `signetry_provenance`.
`SIGNETRY_MCP_ROOTS` scopes the server to your workspace so it can't be pointed at
arbitrary host paths.

## 2. Project rule (defense in depth)

Drop `signetry.mdc` into `.cursor/rules/` so the agent is told to stay within the
contract and to run `signetry guard` before writing forbidden paths. This is advisory
(the model may still err) — the durable guard is running Signetry in CI on the PR via
the [Signetry Admission GitHub Action](https://github.com/marketplace/actions/signetry-admission).

> Note: Cursor has no deterministic pre-write hook like Claude Code, so in Cursor
> the strong enforcement is the CI check on the PR; the MCP tools + rule give the
> agent an in-editor way to self-check first.

---

Part of the [Signetry platform](https://github.com/Signetry/signetry). Governance logic lives in [signetry-core](https://github.com/Signetry/core); this integration never reimplements policy and never auto-merges.

## License

[Apache-2.0](LICENSE). Use it, fork it, ship it commercially — no strings.

This repository is part of Signetry's [open-core model](https://github.com/Signetry/signetry/blob/main/LICENSING.md):
the **integration surface is Apache-2.0** so anyone can add an agent, an editor, or a
CI adapter, while the engine ([`Signetry/core`](https://github.com/Signetry/core)) is
source-available under BUSL-1.1 and converts to Apache-2.0 on 2030-08-31.

Contributions are accepted under the [CLA](CLA.md) — it lets us move a well-built
adapter into the engine later without asking every contributor for permission again.
