# 🧩 AIMETON MCP Infrastructure

Центральный репозиторий контрактов, конфигурации и эксплуатационного состояния MCP-серверов AIMETON.

## Назначение

Этот репозиторий хранит **канонический описательный и эксплуатационный контракт MCP-контура**, но статус каждого MCP endpoint должен подтверждаться live evidence. Историческая запись адреса или порта не является доказательством текущей готовности.

## GitHub MCP Server

Связанная реализация исторически описана через `aimeton_deepseek_integration`.

Исторически зафиксированный endpoint:

```text
http://195.209.215.159:8082/mcp
```

Исторически указанная схема аутентификации: bearer token / GitHub PAT.

**Текущий нормативный статус этого plaintext endpoint: `UNVERIFIED / NOT AUTHORIZED FOR CREDENTIALLED USE`.**

Причина: передача GitHub PAT или bearer token по незашифрованному HTTP запрещена правилами AIMETON. Нельзя использовать этот адрес для privileged/read/write GitHub операций, пока не подтверждён HTTPS/TLS либо защищённый приватный туннель и не выполнен live read-back endpoint/auth/capabilities.

Полный контракт: [`docs/contracts/GITHUB_MCP_ACCESS_CONTRACT.md`](docs/contracts/GITHUB_MCP_ACCESS_CONTRACT.md).

## Канонический fallback GitHub

При отказе одного канала агент не объявляет системный blocker. Используется порядок:

```text
built-in GitHub connector/API
→ alternate operation of the same connector
→ verified secure AIMETON GitHub MCP
→ existing AIMETON command-router / issue-comment bridge
→ GitHub REST/GraphQL/gh through trusted AIMETON server contour
→ owner action only for genuine authority/judgement boundary
```

Если GitHub MCP не имеет подтверждённого secure transport, он **пропускается**, а агент немедленно переходит к следующему безопасному каналу. Отсутствие готовности одного MCP не означает отсутствие GitHub capability у AIMETON.

Канонический общий протокол отказов: `Dimar4713/aimeton-architecture/docs/operations/AGENT_GITHUB_EXECUTION_BRIDGE.md`.

## Источники истины

- `aimeton-architecture` — нормативные общесистемные правила и execution bridge;
- `aimeton-mcp-infra` — MCP registry/contracts/readiness evidence;
- `aimeton-infrastructure` — trusted server contour, command routers, deployment/runtime state;
- интеграционные репозитории — client-specific compatibility evidence.

## Текущая фактическая структура репозитория

```text
.
├── AGENTS.md
├── README.md
├── .github/
└── docs/
    ├── contracts/
    │   ├── GITHUB_MCP_ACCESS_CONTRACT.md
    │   └── RUNTIME_TIME_MCP_CONTRACT.md
    └── roadmap/
```

Старые ссылки на `deploy.md`, `configs/github-mcp.json`, `scripts/deploy.sh` и `docs/troubleshooting.md` не должны считаться существующими файлами, пока они фактически не появятся в репозитории.

## Security invariants

1. PAT, bearer tokens, API keys и private keys не коммитятся и не выводятся в logs/evidence.
2. Credentialled MCP over plaintext HTTP запрещён.
3. Endpoint availability, secure transport, auth validity, capability set и client compatibility проверяются отдельно.
4. Статус `READY` разрешён только при датированном live evidence с endpoint, transport, implementation/version/SHA и наблюдаемым результатом.
5. Если текущий клиент/connector не видит private repository, это ограничение конкретного channel/token, а не системы AIMETON.

## Обновления

- **2026-08-30**: plaintext GitHub MCP documentation переведена из безусловного `работает` в fail-closed operational status; добавлен access/readiness contract.
- **2026-07-27**: репозиторий создан, добавлена начальная документация.

## Лицензия

MIT
