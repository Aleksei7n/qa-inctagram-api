# QA Case — Inctagram API (Postman/Newman)

Портфолио-кейс по тестированию REST API “Inctagram / Somegram”: тест-план, чек-листы, тест-кейсы, баг-репорты, отчёт, матрица покрытия + Postman артефакты.

Источник эндпоинтов: Postman коллекция (Swagger-based) — см. `08-postman/collection.json`.

---

## Stack
- Manual API testing: Postman
- CLI runs: Newman
- Reports: Newman HTML/JSON (пример команд — в `08-postman/newman-run.md`)
- Документация: Markdown

---

## Scope
- Auth: registration, confirmation, login, refresh-token, logout, password recovery, new password, GitHub callback
- Devices: list, terminate device, terminate all
- Users: profile CRUD, avatar CRUD, change email + confirmation, change password, follow/unfollow, followers/following lists
- Posts: CRUD, like/unlike, list by user
- Public: public users profile, public posts feed/profile, following feed
- Notifications, Subscriptions, Country catalog
- WSS endpoints (smoke / availability)

---

## Repository navigation (quick links)
- Requirements / API inventory: `01-requirements/api-requirements.md`
- API Test Plan: `02-test-plan/api-test-plan.md`
- Checklists: `03-checklists/`
- Test Cases: `04-test-cases/api-test-cases.md`
- Bug Reports: `05-bug-reports/`
- Test Summary Report: `06-reports/test-summary-report.md`
- Traceability Matrix: `07-traceability/traceability-matrix.md`
- Postman/Newman: `08-postman/`

---

## How to run (quick)
1) Put your Postman collection into: `08-postman/collection.json`
2) Create environment from: `08-postman/environment.example.json` (set `baseUrl`)
3) Run via Newman — see: `08-postman/newman-run.md`
