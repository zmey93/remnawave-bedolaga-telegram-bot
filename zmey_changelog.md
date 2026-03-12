# ZMEY CHANGELOG — remnawave-bedolaga-telegram-bot (fork)

История изменений форка [`zmey93/remnawave-bedolaga-telegram-bot`](https://github.com/zmey93/remnawave-bedolaga-telegram-bot)
относительно оригинала [`BEDOLAGA-DEV/remnawave-bedolaga-telegram-bot`](https://github.com/BEDOLAGA-DEV/remnawave-bedolaga-telegram-bot).

Формат тегов: `v{upstream}z{zmey}` — например `v3.31.0z0.1`

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
