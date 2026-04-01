# ZMEY CHANGELOG — remnawave-bedolaga-telegram-bot (fork)

История изменений форка [`zmey93/remnawave-bedolaga-telegram-bot`](https://github.com/zmey93/remnawave-bedolaga-telegram-bot)
относительно оригинала [`BEDOLAGA-DEV/remnawave-bedolaga-telegram-bot`](https://github.com/BEDOLAGA-DEV/remnawave-bedolaga-telegram-bot).

Формат тегов: `v{upstream}z{zmey}` — например `v3.31.0z0.1`

---

## [v3.43.1z0.3] — 2026-04-01

### Upstream
- База: [`BEDOLAGA-DEV/remnawave-bedolaga-telegram-bot v3.43.1`](https://github.com/BEDOLAGA-DEV/remnawave-bedolaga-telegram-bot/releases/tag/v3.43.1)
- Слито из upstream `v3.32.3` → `v3.43.1` (~60+ коммитов, ~200 файлов)

### Новые платёжные системы
- **SeverPay** — добавлена полная интеграция: сервис, обработчики, таблица `severpay_payments` в БД (миграция `0040`)
- **RioPay** — поддержка гостевых платежей и лендингов: `user_id` теперь nullable (миграция `0039`)
- **KassaAI** — добавлены подметоды **СБП** и **банковская карта** с единой конфигурацией через словарь

### Новые функции
- **Медиавложения в тикетах** — операторы теперь могут прикреплять фото/видео в ответах на тикеты
- **Раздельные топики** — отдельные форумные топики для каждого типа админских уведомлений
- **Новости/блог в кабинете** — таблицы `news_articles`, `news_categories`, `news_tags` (миграции `0046`, `0049`); папка `uploads` для медиафайлов
- **Граф реферальной сети** — API-эндпоинт для визуализации реферального дерева в админке
- **Deep-link авторизация** — вход в кабинет через прямую ссылку при блокировке `oauth.telegram.org`
- **Поиск платежей** — расширенные фильтры и статистика в панели администратора
- **Мульти-подписки (фундамент)** — снято ограничение 1 подписка на пользователя, подготовлена инфраструктура (миграция `0050`): составные индексы, частичный уникальный индекс по `(user_id, tariff_id)` для `active/trial`
- **Промокоды с привязкой к тарифу** — новое поле `tariff_id` у промокодов (миграция `0052`)

### Исправления
- Исправлен расчёт реферального бонуса инвайтера (сумма вместо максимума)
- Исправлена конверсия в статистике продаж
- Phantom-пользователи объединяются с основным аккаунтом при `/start`
- Автоплатёж теперь учитывает индивидуальный флаг `autopay_enabled` пользователя
- Исправлена верификация вебхуков CryptoBot и Platega
- Исправлена MissingGreenlet ошибка при покупке/смене тарифа/устройств
- Ручные пополнения (admin top-ups) теперь сохраняются с `payment_method='manual'` — исправление данных (миграция `0044`)
- UUID фискальных чеков NaloGO сохраняются на уровне `guest_purchases` (миграция `0045`)

### Производительность БД (миграции `0041`–`0043`, `0047`–`0048`)
| Миграция | Что делает |
|----------|------------|
| `0041` | `CONCURRENTLY` индексы для реферальных запросов: `advertising_campaign_registrations(user_id, created_at)` и `transactions(user_id, type, is_completed, amount_kopeks)` |
| `0042` | Добавлен `retry_count` в `guest_purchases`; выражения-индексы по `metadata_json->>'purchase_token'` для восстановления платежей во всех платёжных таблицах |
| `0043` | Индексы для RBAC: `user_roles(role_id)`, `access_policies(role_id)`; функциональный индекс `lower(email)` |
| `0047` | `ON DELETE CASCADE` для `subscription_servers.subscription_id` + индекс |
| `0048` | Функциональный индекс `lower(username)` для поиска phantom-пользователей |

### Изменения в форке (`zmey93`)
- **`docker-compose.yml`**: добавлен том `uploads` для медиафайлов новостей с поддержкой env-переменных `${BOT_UPLOADS_HOST_PATH:-./uploads}` / `${BOT_UPLOADS_CONTAINER_PATH:-/app/uploads}`

### ⚠️ Обязательные действия при обновлении
1. **Прокатить миграции**: `alembic upgrade head` (миграции `0039`–`0052`)
2. **Создать папку** `uploads` на сервере рядом с `docker-compose.yml` (или указать путь через `BOT_UPLOADS_HOST_PATH`)
3. Опционально добавить в `.env`:
   ```env
   BOT_UPLOADS_HOST_PATH=./uploads
   BOT_UPLOADS_CONTAINER_PATH=/app/uploads
   ```

---

## [v3.31.0z0.2] — 2026-03-12

### Upstream
- База: [`BEDOLAGA-DEV/remnawave-bedolaga-telegram-bot v3.31.0`](https://github.com/BEDOLAGA-DEV/remnawave-bedolaga-telegram-bot/releases/tag/v3.31.0)

### Исправлено
- **`.github/workflows/docker-hub.yml`**: отключён — в форке нет секретов `DOCKER_USERNAME`/`DOCKER_PASSWORD`, workflow падал с ошибкой `Username and password required`
- **`.github/workflows/docker-registry.yml`**: исправлен — логин теперь через `GITHUB_TOKEN` в GHCR (не Docker Hub), версия снапшотов использует формат `v3.31.0z0.1-{sha}` (убран хардкод `v3.7.0` из апстрима)
- **`IMAGE_NAME`**: захардкожен как `zmey93/remnawave-bedolaga-telegram-bot` (раньше был `${{ github.repository }}` — мог подставить чужой орг)

---

## [v3.31.0z0.1] — 2026-03-12

### Upstream
- База: [`BEDOLAGA-DEV/remnawave-bedolaga-telegram-bot v3.31.0`](https://github.com/BEDOLAGA-DEV/remnawave-bedolaga-telegram-bot/releases/tag/v3.31.0)

### Добавлено в форке
- **`.github/workflows/`**: настроен CI/CD — автобилд Docker в GHCR, авторелиз `zmey-release.yml` с форматом `vX.Y.ZzN`
- **`release-please.yml`**: отключён

---

<!-- Новые секции добавляй выше этой строки в формате:
## [vX.Y.ZzN] — YYYY-MM-DD
### Upstream
- База: BEDOLAGA-DEV v...
### Исправлено / Добавлено
- ...
-->
