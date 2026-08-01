# Umbra for Cursor

> **Copyright (c) 2026 Binay Dalai. All rights reserved.**
> This repository is strictly for viewing and contributing to the original project. You may not use, copy, modify, distribute, or commercialize this code for your own personal or commercial projects without explicit written permission. Only the original author retains the right to use and monetize this project.


Two ways to govern coding-agent changes in Cursor with
[umbra-core](https://github.com/bkd-dotcom/umbra-core).

## Prerequisite

```bash
pip install "umbra-core>=0.3.0"
```

Add a `.umbra/admission.yaml` to your repo declaring allowed/forbidden paths,
diff budget, and required checks (a conservative default applies without one).

## 1. MCP server (recommended)

Cursor speaks MCP. Add Umbra's server so the agent can run admission / verify /
provenance itself. Copy `mcp.json` to `.cursor/mcp.json` in your project (or merge
into your existing one):

```json
{
  "mcpServers": {
    "umbra": {
      "command": "python",
      "args": ["-m", "umbra_core.mcp_server"],
      "env": { "UMBRA_MCP_ROOTS": "${workspaceFolder}" }
    }
  }
}
```

Then the agent can call `umbra_admit`, `umbra_verify`, and `umbra_provenance`.
`UMBRA_MCP_ROOTS` scopes the server to your workspace so it can't be pointed at
arbitrary host paths.

## 2. Project rule (defense in depth)

Drop `umbra.mdc` into `.cursor/rules/` so the agent is told to stay within the
contract and to run `umbra guard` before writing forbidden paths. This is advisory
(the model may still err) — the durable guard is running Umbra in CI on the PR via
the [Umbra Admission GitHub Action](https://github.com/marketplace/actions/umbra-admission).

> Note: Cursor has no deterministic pre-write hook like Claude Code, so in Cursor
> the strong enforcement is the CI check on the PR; the MCP tools + rule give the
> agent an in-editor way to self-check first.

---

Part of the [Umbra platform](https://github.com/bkd-dotcom/umbra-umbrella). Governance logic lives in [umbra-core](https://github.com/bkd-dotcom/umbra-core); this integration never reimplements policy and never auto-merges.

## License

**Copyright (c) 2026 Binay Dalai. All rights reserved.** This code is not open source. You may not use, copy, modify, distribute, or commercialize it for your own personal or commercial purposes without explicit written permission from the author, who alone retains the right to use and monetize this project. See [CONTRIBUTING.md](CONTRIBUTING.md).
