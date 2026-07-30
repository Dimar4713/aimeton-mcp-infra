# AIMETON MCP hardening plan

Status: approved for implementation
Updated: 2026-07-31

## Goal

Turn the existing GitHub MCP endpoint into a secure, observable and reusable second GitHub control plane for AIMETON agents.

## Current state

- Streamable HTTP endpoint is published on port 8082.
- Client authentication currently reuses a GitHub PAT.
- The public endpoint is documented with plain HTTP.
- The MCP server is not automatically available in every ChatGPT session.

## Target state

1. Publish the service only through TLS, preferably `https://mcp.aimeton.ru/mcp`, behind Caddy.
2. Replace direct client-side GitHub PAT transport with a dedicated MCP access token; keep the GitHub credential only in server-side secret storage.
3. Restrict network exposure of port 8082 to localhost or the private Docker network.
4. Add health, readiness and capability discovery endpoints without exposing secrets.
5. Add structured audit logs for caller, tool, repository, result and request correlation ID; redact credentials and sensitive payloads.
6. Define least-privilege GitHub credentials and a documented rotation/revocation procedure.
7. Add rate limiting, request size limits, timeouts and fail-closed authentication.
8. Add automated deployment, rollback and live smoke checks.
9. Publish a machine-readable tool inventory and document the fallback chain: built-in GitHub connector → AIMETON GitHub MCP → direct REST/GraphQL/gh → manual UI.
10. Add an integration path so supported AI clients can connect without manually reconstructing endpoint and headers.

## Stages

### P0 — transport and credential safety

- TLS reverse proxy and DNS.
- Close direct public access to `:8082`.
- Dedicated MCP token.
- Server-side GitHub secret.
- Rotate the GitHub PAT after migration.

Acceptance: no GitHub credential crosses an unencrypted channel; direct access to port 8082 from the public Internet is denied.

### P1 — operability

- Health/readiness/capability probes.
- Structured redacted audit logging.
- Rate limits, timeouts and request limits.
- Deployment and rollback automation.

Acceptance: deployment and a real read-only GitHub tool call pass an automated live smoke and produce redacted evidence.

### P2 — agent integration

- Client configuration templates.
- Tool inventory and version contract.
- Permission profiles for read-only, repository-write and Actions operations.
- Documented fallback and incident procedure.

Acceptance: a fresh supported client can connect using a documented configuration and invoke only the permitted tools.

## Governance

Implementation evidence must include configuration diff, security checks, live smoke, credential-rotation confirmation and rollback proof. Secrets must never be committed or copied into issues, logs or chat.