# QA Case — Inctagram API (Postman/Newman)

Портфолио-кейс по тестированию REST API **Inctagram / Somegram**: требования/инвентарь, тест-план, чек-листы, тест-кейсы, баг-репорты, отчёт, матрица покрытия + Postman/Newman артефакты.

Источник эндпоинтов: Postman-коллекция (Swagger-based) — `08-postman/collection.json`.

---

## Stack
- Manual API testing: **Postman**
- CLI runs: **Newman**
- Reports: **Newman HTML/JSON** (пример команд — `08-postman/newman-run.md`)
- Документация: **Markdown**

---

## Scope (что покрыто)
- **Auth:** registration, confirmation, login, refresh-token, logout, password recovery, new password, GitHub callback
- **Devices:** list sessions, terminate by deviceId, terminate all
- **Users:** profile read/update, avatar upload/delete, change email + confirmation, change password, follow/unfollow, followers/following lists
- **Posts:** create/read/update/delete, like/unlike, list by user
- **Public:** public users profile, public posts feed/profile, following feed
- **Notifications**, **Subscriptions**, **Country catalog**
- **WSS:** smoke/availability (handshake)

---

## Repository structure
- `01-requirements/` — требования и инвентарь API (на основе коллекции)
- `02-test-plan/` — тест-план
- `03-checklists/` — smoke/regression/security sanity чек-листы
- `04-test-cases/` — тест-кейсы по модулям (Auth/Users/Posts/…)
- `05-bug-reports/` — баг-репорты (пример: `BUG.md` + отдельные BUG-файлы при расширении)
- `06-reports/` — Test Summary Report + (опционально) newman-отчёты
- `07-traceability/` — матрица покрытия (requirements → test cases)
- `08-postman/` — Postman коллекция, окружение, snippets, newman run, CI примеры

---

## Navigation (quick links)
- Requirements / API inventory: `01-requirements/api-requirements.md`
- API Test Plan: `02-test-plan/api-test-plan.md`
- Checklists: `03-checklists/`
- Test Cases: `04-test-cases/` (файлы по модулям: auth/users/posts/…)
- Bug Reports: `05-bug-reports/BUG.md`
- Test Summary Report: `06-reports/test-summary-report.md`
- Traceability Matrix: `07-traceability/traceability-matrix.md`
- Postman/Newman: `08-postman/README.md`

---

## How to run (quick)
1) Открой `08-postman/collection.json` в Postman (Import).
2) Импортируй окружение `08-postman/environment.example.json` и заполни:
   - `baseUrl` — обязателен
   - `bearerToken` — оставь пустым (заполнится после login, если используешь snippet)
3) Для примеров автопроверок смотри `08-postman/postman-tests-snippets.md`.
4) Прогон через CLI: команды в `08-postman/newman-run.md`.

---

## Notes
- Не коммить реальные токены/пароли/почты — используй environment/Secrets.
- Часть сценариев (confirmation/recovery) может зависеть от внешнего mail сервиса и быть ограничена на стенде.
