# 🧩 AIMETON MCP Infrastructure

Центральный репозиторий для управления MCP-серверами, их конфигурацией и документацией.

## 📌 Назначение

Этот репозиторий содержит всё необходимое для развертывания и поддержки MCP-серверов в экосистеме AIMETON.

## 🚀 Текущие MCP-серверы

### GitHub MCP Server

- **Репозиторий**: [aimeton_deepseek_integration](https://github.com/Dimar4713/aimeton_deepseek_integration)
- **Транспорт**: Streamable HTTP
- **Порт**: 8082
- **Аутентификация**: Bearer Token (GitHub PAT)
- **Статус**: ✅ Работает

## 🔌 Подключение в DeepSeek++

```
Транспорт: Streamable HTTP
URL: http://195.209.215.159:8082/mcp
Headers: Authorization: Bearer <GITHUB_PAT>
```

## 📖 Документация

- [README.md](README.md) — общая информация
- [deploy.md](deploy.md) — инструкция по деплою на VPS

## 📁 Структура репозитория

```
.
├── README.md              # Этот файл
├── deploy.md              # Инструкция по деплою
├── configs/               # Конфигурационные файлы
│   └── github-mcp.json    # Конфиг для GitHub MCP
├── scripts/               # Скрипты для управления
│   └── deploy.sh          # Скрипт деплоя
└── docs/                  # Документация
    └── troubleshooting.md # Решение проблем
```

## 🔗 Связанные репозитории

- [aimeton-architecture](https://github.com/Dimar4713/aimeton-architecture) — архитектурные решения
- [aimeton_deepseek_integration](https://github.com/Dimar4713/aimeton_deepseek_integration) — контекст и память DeepSeek
- [aimeton-infrastructure](https://github.com/Dimar4713/aimeton-infrastructure) — инфраструктурные ресурсы (закрыт)

## 📝 Обновления

- **2026-07-27**: Репозиторий создан, добавлена начальная документация

## 📄 Лицензия

MIT
