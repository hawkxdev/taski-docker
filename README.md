# Taski

Приложение для управления задачами (to-do). Backend на Django REST Framework, frontend на React, контейнеризация через Docker.

## Стек технологий

- [Python 3.12](https://docs.python.org/3.12/)
- [Django 3.2](https://docs.djangoproject.com/en/3.2/)
- [Django REST Framework 3.12](https://www.django-rest-framework.org/)
- [React](https://react.dev/)
- [Node.js 18](https://nodejs.org/)
- [PostgreSQL 13](https://www.postgresql.org/docs/13/)
- [Nginx 1.22](https://nginx.org/)
- [Gunicorn 23](https://gunicorn.org/)
- [Docker](https://docs.docker.com/)
- [GitHub Actions](https://docs.github.com/en/actions) (CI/CD)

## Архитектура

| Nginx :80 | → | Backend :8000 | → | PostgreSQL :5432 |
|-----------|---|---------------|---|------------------|

Nginx также раздаёт `/staticfiles/` (frontend build + backend static).

## Необходимое ПО

- [Docker](https://docs.docker.com/get-docker/) >= 20.10
- [Docker Compose](https://docs.docker.com/compose/install/) >= 2.0

## Установка и запуск (Docker)

### Клонирование

```bash
git clone git@github.com:hawkxdev/taski-docker.git
cd taski-docker
```

### Переменные окружения

Создайте файл `.env` в корне проекта:

```bash
POSTGRES_USER=django_user
POSTGRES_PASSWORD=your_password
POSTGRES_DB=django
DB_HOST=db
DB_PORT=5432
```

### Запуск контейнеров

Для локальной разработки:

```bash
docker compose up --build -d
```

Для production:

```bash
docker compose -f docker-compose.production.yml up -d
```

### Миграции и статика

```bash
docker compose exec backend python manage.py migrate
docker compose exec backend python manage.py collectstatic --no-input
```

### Создание суперпользователя

```bash
docker compose exec backend python manage.py createsuperuser
```

### Доступ

- Приложение: http://localhost:8000
- Админ-панель: http://localhost:8000/admin/
- API: http://localhost:8000/api/

## Структура проекта

```
taski-docker/
├── backend/          # Django-приложение (API + админка)
│   ├── api/          # REST API
│   ├── backend/      # Настройки Django
│   └── Dockerfile
├── frontend/         # React-приложение
│   ├── src/
│   └── Dockerfile
├── gateway/          # Nginx-конфигурация
│   ├── nginx.conf
│   └── Dockerfile
├── docker-compose.yml              # Локальная разработка
├── docker-compose.production.yml   # Production
└── .github/workflows/main.yml      # CI/CD
```

## CI/CD

При пуше в ветку `main` GitHub Actions автоматически:

1. Запускает тесты backend (flake8 + Django tests)
2. Запускает тесты frontend (npm test)
3. Собирает и пушит Docker-образы на Docker Hub
4. Деплоит на production-сервер
5. Отправляет уведомление в Telegram

## Автор

[hawkxdev](https://github.com/hawkxdev)

## Лицензия

[MIT](LICENSE)

---

Проект создан в рамках учебного курса [Яндекс Практикум](https://practicum.yandex.ru/).
