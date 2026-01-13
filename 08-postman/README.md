# Postman / Newman — How to run (Inctagram/Somegram API)

Версия: 1.1  

---

## Files
- `collection.json` — Postman collection (источник правды по эндпоинтам)
- `environment.example.json` — пример окружения (без секретов)
- `postman-tests-snippets.md` — примеры автопроверок (pm.test)
- `newman-run.md` — команды запуска newman + отчёт
- `github-actions-newman.yml` — пример CI (GitHub Actions)
- `evidence/` — (опционально) сохранённые ответы/примеры для багов/отчёта

---

## Postman setup
1) Импортировать коллекцию:
   - Postman → Import → выбрать `08-postman/collection.json`
2) Импортировать окружение:
   - Import → выбрать `08-postman/environment.example.json`
3) Заполнить переменные в Environment:
   - `baseUrl` (обязательно)
   - `bearerToken` (оставить пустым — заполнится из login)

---

## Notes (security)
- Не коммить реальные токены/пароли/почты.
- Для CI использовать GitHub Secrets (например `BASE_URL`) и подстановку в env на раннере.
