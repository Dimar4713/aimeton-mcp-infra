# AIMETON Runtime Time MCP Contract

Status: approved implementation contract
Canonical law: `Dimar4713/aimeton-architecture/docs/standards/RUNTIME_TIME_STANDARD.md`

## Tool

Canonical read-only tool name:

`runtime.time`

Optional compatibility alias during migration:

`time.utcnow`

## Input

No arguments. Mutation, timezone selection and client-supplied timestamps are not supported.

## Output

```json
{
  "utc": "2026-08-05T00:00:00.000Z",
  "unix_ms": 0,
  "source": "chrony",
  "synced": true,
  "offset_ms": 0.0,
  "stratum": 2,
  "quality": "trusted",
  "reason_code": null
}
```

## Source of truth

1. The MCP server MUST call the approved AIMETON Runtime Time API or an equivalent local trusted adapter.
2. It MUST NOT use the LLM clock, browser clock or caller clock as canonical time.
3. A local process-clock fallback is allowed only when explicitly returned as `quality=fallback` with a bounded reason code.
4. The MCP server MUST NOT disclose NTP server lists, hostnames, private addresses, filesystem paths or command output.

## Reliability and safety

- bounded timeout;
- no retries beyond the approved small retry budget;
- read-only and idempotent;
- fail explicit, never silently replace trusted time with local time;
- emit UMEL events `provider.requested`, `provider.responded`, `service.degraded` or `mission.failed` where appropriate;
- all MCP logs use canonical UTC when available and mark fallback otherwise.

## Acceptance

- output schema validation;
- synchronized trusted path;
- unavailable API path;
- excessive offset/degraded path;
- malformed upstream payload;
- no secret/path leakage;
- correlation timestamp matches Runtime Time API within policy tolerance.

## Tactical plan

1. Add typed schema and tool registration.
2. Add configurable Runtime Time API URL via non-secret environment configuration.
3. Add bounded client, redaction and tests.
4. Publish tool in MCP capability manifest.
5. Add integration acceptance against stage.
6. Migrate agent telemetry and durable handoff helpers to `runtime.time`.
