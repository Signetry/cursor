# Contributing

Thanks for wanting to improve the Cursor integration. This repository is
**[Apache-2.0](LICENSE)** — you may use, modify, fork, redistribute, and ship it
commercially, with no permission needed from anyone.

## What lives here

This repo is small on purpose. It is the Cursor-side surface over
[signetry-core](https://github.com/Signetry/core), which owns *all* governance logic:

| File | What it is |
|---|---|
| `mcp.json` | The MCP server entry a user copies to `.cursor/mcp.json`, exposing `signetry_admit` / `signetry_verify` / `signetry_provenance` to the agent. |
| `signetry.mdc` | The Cursor project rule a user drops into `.cursor/rules/` — advisory guidance to stay inside the admission contract. |
| `README.md` | Setup for both paths. |

Two rules for changes here: this integration **never reimplements policy** (that
belongs in `signetry-core`), and it **never auto-merges** — `auto_merge` is always
false; a human approves the PR. A change that breaks either of those will not be
merged. If the behavior you want lives in the engine, open the issue or PR on
[Signetry/core](https://github.com/Signetry/core) instead.

## Getting started

There is no build step and no test suite in this repository — it is configuration
and documentation. To exercise a change end to end, install the engine and point
Cursor at your working copy:

```bash
pip install "signetry-core @ git+https://github.com/Signetry/core@v0.7.0"
```

Then, in a scratch project with a `.signetry/admission.yaml`:

- copy your edited `mcp.json` to `.cursor/mcp.json` and confirm the agent can call
  `signetry_admit`, `signetry_verify`, and `signetry_provenance`;
- copy your edited `signetry.mdc` to `.cursor/rules/` and confirm Cursor loads it
  (the YAML frontmatter must stay valid, with `alwaysApply` intact);
- if you touched the guard advice, check the commands it tells the agent to run
  still exist: `signetry guard --path <file>`, `signetry guard --command "<cmd>"`,
  and `signetry admit . --agent none --mission "<what you changed>"`.

Say in the pull request which of these you actually ran.

## What CI does on your PR

- **CLA** (`.github/workflows/cla.yml`) — blocks the merge until you have signed
  (see below).
- **Reviewer** (`.github/workflows/reviewer.yml`) — an advisory
  [signetry-reviewer](https://github.com/Signetry/reviewer) pass that posts one
  recommendation comment. It is advisory only: it never merges and never fails your
  PR. It does flag changes to security-sensitive surfaces (workflows, packaging) for
  a human to look at.

## Signing the CLA (required before merge)

This is enforced by a bot. When you open a pull request, the **CLA Assistant** check
will ask you to sign the [Contributor License Agreement](CLA.md). Reply on the PR
with exactly:

```
I have read the CLA Document and I hereby sign the CLA
```

Your acceptance is recorded in `signatures/cla.json`. A PR **cannot be merged** until
the CLA is signed.

**Why a CLA on an Apache-2.0 repo?** Because Signetry is
[open core](https://github.com/Signetry/signetry/blob/main/LICENSING.md), and code
sometimes has to move across that line: an adapter that proves itself here may belong
in the engine ([`Signetry/core`](https://github.com/Signetry/core)), which is
BUSL-1.1 until it converts to Apache-2.0 on 2030-08-31. The CLA lets that happen
without tracking down every past contributor for permission. It does **not** take
your copyright — you keep it, and grant a licence alongside it. Your contribution
also stays available to you and to everyone else under Apache-2.0, because that is
the licence this repository ships under.

## Credit

Contributors are acknowledged in [CONTRIBUTORS.md](CONTRIBUTORS.md), the Git history,
and release notes. Add yourself in the same PR if you like, or leave it to a
maintainer.

## Reporting a vulnerability

Do not open a public issue. See [SECURITY.md](SECURITY.md) — vulnerabilities in the
governance pipeline itself belong on
[signetry-core](https://github.com/Signetry/core).
