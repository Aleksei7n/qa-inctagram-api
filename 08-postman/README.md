# Postman / Newman — How to run (Inctagram/Somegram API)

Версия: 1.2  
Проект: **qa-inctagram-api**  
Источник эндпоинтов: Postman collection `collection.json` (v1)  
Переменные окружения: `{{baseUrl}}`, `{{bearerToken}}`

---

## Структура папки `08-postman/`

`08-postman/`
- `README.md` — (как запускать и как хранить)
- `collection.json` — Postman collection
- `environment.example.json` — пример окружения
- `postman-tests-snippets.md` — 10 готовых блоков проверок для вкладки **Tests** (включая Helpers)

---

## Быстрый старт в Postman

### 1) Импорт коллекции
Postman → **Import** → выбрать файл:  
`08-postman/collection.json`

### 2) Импорт окружения
Postman → **Import** → выбрать файл:  
`08-postman/environment.example.json`

### 3) Настрой переменные окружения (Environment)
- `baseUrl` — **обязательно** (пример: `https://<host>`)
- `bearerToken` — оставь пустым (заполнится после логина)
