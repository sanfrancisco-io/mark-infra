# Session 01 — Базовая инфраструктура

**Дата:** 2026-03-25 — 2026-03-26
**Статус:** Завершена

## Цель

Поднять базовую Docker Compose инфраструктуру для marketplace с нуля: PostgreSQL, MinIO, backend и frontend placeholder-ы.

## Что сделано

### Фаза 1 — Docker Compose базовый (2026-03-25)
- Создан `docker-compose.yml` с 5 сервисами: postgres, minio, minio-init, backend, frontend
- Настроены volume mapping: `./data/postgres` и `./data/minio`
- Создан `.env.example` с DATABASE_URL (asyncpg), MINIO_*, BACKEND_PORT, FRONTEND_PORT
- Создана структура директорий `data/postgres/`, `data/minio/` с `.gitkeep`
- Протестировано (2026-03-26): все 4 сервиса поднялись `Up`

### Фаза 2 — Healthchecks (2026-03-26)
- `postgres`: `pg_isready -U $POSTGRES_USER -d $POSTGRES_DB`, interval 5s, retries 10
- `minio`: `curl -f http://localhost:9000/minio/health/live`, interval 5s, retries 10
- `backend`: `python -c "import urllib.request; urllib.request.urlopen('http://localhost:8000')"`, interval 10s
- `frontend`: без healthcheck (nginx placeholder, достаточно для прототипа)
- `minio-init`: убран `restart: on-failure` и retry-петля, заменён на `depends_on: minio: condition: service_healthy`
- `backend` ждёт postgres и minio через `condition: service_healthy`
- `frontend` ждёт backend через `condition: service_healthy`
- Протестировано: postgres/minio/backend — healthy ✓, frontend — Up ✓

### Фаза 3 — README (2026-03-26)
- Написан `README.md`: быстрый старт, таблица сервисов, переменные окружения, команды управления

## Принятые решения

| Решение | Обоснование |
|---|---|
| `PGDATA` env + вложенный путь `/var/lib/postgresql/data/pgdata` | Избегает конфликта с системными файлами postgres при маппинге volume |
| `minio-init` как отдельный one-shot контейнер | Изолирует логику создания bucket, не загрязняет основной сервис |
| `urllib.request` вместо `curl` для healthcheck backend | `python:3.12-slim` не содержит curl; использование встроенных средств языка |
| DATABASE_URL как единая строка | Удобнее для FastAPI/SQLAlchemy, не нужно собирать из частей |

## Итог

Инфраструктура готова к подключению реальных backend и frontend сервисов.
