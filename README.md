# marketplace-infra

Docker Compose инфраструктура для marketplace: PostgreSQL, MinIO, backend, frontend.

## Требования

- [Docker](https://docs.docker.com/get-docker/) с поддержкой Compose v2 (`docker compose`)

## Быстрый старт

```bash
git clone <repo-url>
cd marketplace-infra

cp .env.example .env
# При необходимости отредактируй .env (пароли, порты)

docker compose up -d
```

Через ~30 секунд все сервисы будут готовы:

```bash
docker compose ps
```

## Сервисы

| Сервис    | Адрес                         | Описание                        |
|-----------|-------------------------------|---------------------------------|
| postgres  | localhost:5432                | PostgreSQL 15                   |
| minio     | http://localhost:9000         | S3 API                          |
| minio     | http://localhost:9001         | Web-консоль MinIO               |
| backend   | http://localhost:8000         | Backend API (placeholder)       |
| frontend  | http://localhost:3000         | Frontend (placeholder)          |

Credentials MinIO по умолчанию: `minioadmin` / `minioadmin`

## Переменные окружения

Все переменные описаны в `.env.example`. Скопируй в `.env` и заполни:

| Переменная          | По умолчанию                                              | Описание                    |
|---------------------|-----------------------------------------------------------|-----------------------------|
| `POSTGRES_USER`     | `marketplace`                                             | Пользователь БД             |
| `POSTGRES_PASSWORD` | `secret`                                                  | Пароль БД                   |
| `POSTGRES_DB`       | `marketplace`                                             | Имя базы данных             |
| `DATABASE_URL`      | `postgresql+asyncpg://marketplace:secret@postgres:5432/marketplace` | URL для подключения (asyncpg) |
| `MINIO_ROOT_USER`   | `minioadmin`                                              | Логин MinIO                 |
| `MINIO_ROOT_PASSWORD` | `minioadmin`                                            | Пароль MinIO                |
| `MINIO_BUCKET`      | `products`                                                | Имя bucket (публичный)      |
| `BACKEND_PORT`      | `8000`                                                    | Внешний порт backend        |
| `FRONTEND_PORT`     | `3000`                                                    | Внешний порт frontend       |

## Управление

```bash
# Запустить в фоне
docker compose up -d

# Посмотреть статус и healthchecks
docker compose ps

# Логи конкретного сервиса
docker compose logs -f backend

# Остановить (данные сохраняются в ./data/)
docker compose down

# Полный сброс с удалением данных
docker compose down && rm -rf data/postgres/* data/minio/*
```

## Данные

Данные хранятся локально и переживают `docker compose down`:

- `./data/postgres/` — файлы PostgreSQL
- `./data/minio/` — файлы объектного хранилища
