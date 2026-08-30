# AIMETON GitHub MCP Access Contract

Status: canonical operational contract

Date: 2026-08-30

## Purpose

Define when the AIMETON GitHub MCP channel may be used as a fallback after a built-in GitHub connector refusal, and prevent one channel/token limitation from being misclassified as an AIMETON system blocker.

## Channel-local failure principle

A failure of one connector, token, MCP endpoint, wrapper or workflow proves only a limitation of that execution channel until independent AIMETON paths are checked.

The following statements are invalid without fallback evidence:

- `private repository is inaccessible`;
- `GitHub cannot do this`;
- `the user must do it manually`;
- `we need a new PAT/secret`;
- `the only path is ...`.

Canonical refusal protocol: `Dimar4713/aimeton-architecture/docs/operations/AGENT_GITHUB_EXECUTION_BRIDGE.md`.

## GitHub MCP readiness dimensions

Readiness is not one boolean. The agent MUST distinguish:

1. **Endpoint reachability** — TCP/HTTP endpoint responds.
2. **Secure transport** — HTTPS/TLS or an approved private encrypted tunnel is verified.
3. **Authentication contract** — bearer/PAT mechanism is known without exposing credential values.
4. **Credential scope** — the credential is authorized for the required repository/action family.
5. **Capability surface** — required read/write/tool operation is actually exposed.
6. **Client compatibility** — the calling client can use the MCP transport/schema.
7. **Live evidence freshness** — status was verified recently against the exact endpoint/implementation.

`READY` requires all dimensions relevant to the requested operation.

## Historical plaintext endpoint

Repository history documents:

```text
http://195.209.215.159:8082/mcp
```

with bearer token / GitHub PAT authentication.

This endpoint MUST be treated as:

```text
transport: plaintext HTTP
credentialled_use: FORBIDDEN
operational_readiness: UNVERIFIED
```

until a secure transport path is documented and live-verified.

Do not send a PAT, bearer token or other secret to a plaintext HTTP endpoint, even for a read-only GitHub operation.

## Fallback state machine

When the built-in GitHub connector refuses or lacks an operation:

```text
1. Classify connector failure
2. Try an alternate supported operation of the same connector where applicable
3. Check GitHub MCP readiness contract
   ├─ secure + live-verified → use it
   └─ not secure/unverified → skip it, do not stop
4. Check existing aimeton-infrastructure command-router / issue-comment bridge
5. Use REST/GraphQL/gh through the trusted AIMETON server contour when authorized
6. Ask owner only for a genuine authority/judgement boundary
```

A skipped unsafe MCP channel does not satisfy the whole fallback search by itself; the next safe channel must be attempted.

## Credential discovery rule

Before requesting a new credential or secret:

1. search repository workflows/scripts/runbooks for **secret/env references** and named credential contracts;
2. inspect existing command-router and trusted-server paths;
3. inspect prior successful runs/evidence;
4. do not attempt to retrieve or print secret values;
5. only after existing paths are disproved may a missing credential be reported.

A `403`/`404` from a repository-scoped `GITHUB_TOKEN` is not proof that no AIMETON cross-repository credential exists.

## Private cross-repository data rule

A public product repository must not create a mutable independent copy of a private infrastructure source-of-truth solely because its ordinary workflow token cannot read the private repository.

Allowed patterns, in preference order:

1. canonical-side generation/synchronization;
2. existing authorized cross-repository credential on secure transport;
3. existing AIMETON command bridge;
4. immutable generated projection carrying canonical repository, exact source SHA, source paths, immutable blob/object identifiers and content digest, plus fail-closed drift verification.

Do not create a second resolver/controller/dispatcher as an access workaround.

## Workflow diagnostic distinction

If a GitHub Actions run fails with **zero jobs created**, classify it first as workflow compile/policy/reusable-workflow accessibility failure. Do not attribute it to self-hosted runner availability.

If a job exists but has no steps and remains queued/in-progress, investigate scheduler/runner placement separately.

## Evidence required before blocker

Before escalating a GitHub access blocker, record:

```text
operation:
connector result:
refusal class:
GitHub MCP secure-readiness result:
command-router result:
trusted REST/GraphQL/gh result:
existing credential-reference search:
falsification attempt:
conclusion:
next safe action:
```

Only a confirmed authority requirement, exhausted secure machine paths, or a domain-policy rejection is a valid owner blocker.

## Current evidence state

As of this contract creation, the repository contains historical documentation of a plaintext GitHub MCP endpoint, but no repository evidence proving HTTPS/TLS or an approved encrypted tunnel for credentialled use. Therefore privileged GitHub MCP use is fail-closed until such evidence is added.

This does not mean AIMETON lacks GitHub access: built-in connectors, infrastructure command-router paths and trusted-server GitHub API paths remain separate capabilities and must be evaluated independently.
