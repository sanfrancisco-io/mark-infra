# TODO — marketplace-infra

## Стек и решения
- Docker Compose v2
- PostgreSQL 15
- MinIO (S3-совместимое хранилище)
- Принятые решения: (заполни после первой сессии)

## Структура проекта
```
marketplace-infra/
  docker-compose.yml
  .env
  .env.example
  data/
    postgres/     ← volume для postgres
    minio/        ← volume для minio
  README.md
  docs/ai/
    AI_WORKFLOW.md
    sessions/
```

## Переменные окружения (.env.example)
```
POSTGRES_USER=marketplace
POSTGRES_PASSWORD=secret
POSTGRES_DB=marketplace

MINIO_ROOT_USER=minioadmin
MINIO_ROOT_PASSWORD=minioadmin
MINIO_BUCKET=products

BACKEND_PORT=8000
FRONTEND_PORT=3000
```

---

## Фаза 1 — Docker Compose базовый ✅ (2026-03-25)

- [x] Создать docker-compose.yml с сервисами: postgres, minio, backend, frontend
- [x] Настроить volume mapping для postgres (./data/postgres:/var/lib/postgresql/data)
- [x] Настроить volume mapping для minio (./data/minio:/data)
- [x] Создать .env.example с переменными: DB_URL, MINIO_*, ADMIN_*
- [x] Проверить: docker compose up поднимает все 4 сервиса

## Фаза 2 — Healthchecks и порядок запуска ✅ (2026-03-26)

- [x] Добавить healthcheck для postgres (pg_isready)
- [x] Добавить healthcheck для minio
- [x] Настроить depends_on с condition: service_healthy для backend
- [x] Настроить depends_on для frontend (ждёт backend)
- [x] Проверить: docker compose ps — все сервисы healthy

## Фаза 3 — README и финальная проверка ✅ (2026-03-26)

- [x] Написать README.md: git clone → cp .env.example .env → docker compose up
- [x] Проверить сценарий с нуля на чистой машине
- [x] Убедиться что данные сохраняются после docker compose down/up

---

## Лог сессий
- (заполняй после каждой сессии)
- [дата] Фаза X — что сделали, сколько итераций, что сломалось
