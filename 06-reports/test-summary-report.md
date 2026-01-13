# Test Summary Report — Inctagram API (v1)

Версия: 1.1  
Проект: Inctagram API  
Окружение по умолчанию: Postman (Desktop), baseUrl={{baseUrl}}  

---

## 1) Build / Date
- baseUrl: `{{baseUrl}}`
- Tooling: Postman / Newman (опционально)
- Collection: `08-postman/collection.json`
- Date: YYYY-MM-DD

---

## 2) Scope Tested (в рамках Test Plan)

### In scope
- **Auth**
  - Registration / Confirmation
  - Login / Refresh token / Logout
  - Password recovery / New password
- **Users (private)**
  - Profile (get/update), avatar (upload/delete), email/password flows
  - Followers/Following (endpoints, где предусмотрено)
- **Posts (private)**
  - CRUD posts, like/unlike
- **Public**
  - Public users / public posts feeds
- **Devices**
  - Get devices, terminate device, terminate all
- **Notifications**
  - Get notifications
- **Country catalog**
  - Countries/regions/cities
- **Subscriptions**
  - Create/cancel/current/payments
- **WSS**
  - Smoke handshake/availability (если endpoint стабильно доступен)

### Out of scope
- Нагрузочное тестирование
- Глубокие проверки производительности и лимитов (rate limit) — только sanity
- Интеграции внешних провайдеров (email) — если недоступны на стенде

---

## 3) Test Results

- Planned: 67  
  - 57 базовых (по 1 позитивному на endpoint/ключевую ручку)
  - + 10 негативных (401/403/validation/invalid id)
- Executed: 67
- Passed: 61
- Failed: 6
- Blocked: 0

---

## 4) Defects Summary (Key Defects)
- **BUG-001** — Logout не инвалидирует accessToken (Critical, P0)
- **BUG-002** — Invalid email в регистрации приводит к 500 вместо 400 (Major, P1)
- **BUG-003** — Возможность удалить/изменить чужой пост (IDOR) (Critical, P0)

---

## 5) Risks / Notes
- Флоу подтверждения регистрации / смены email / recovery может зависеть от внешнего mail-сервиса → возможны ограничения на стенде.
- TTL токенов/refresh-rotation могут вызывать нестабильность при длительном прогоне (refresh/logout).
- Общая БД/тестовые данные могут приводить к конфликтам (409 / duplicate) → требуется стратегия уникальности и очистки.

---

## 6) Recommendations
- Ввести единый формат ошибок (error schema) для 4xx/5xx (исключить 500 на валидациях).
- Закрыть authorization/IDOR для Posts (update/delete) и проверить остальные ресурсы на аналогичный риск.
- Для logout гарантировать инвалидирование accessToken (и refreshToken, если используется).
