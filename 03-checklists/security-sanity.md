# Security Sanity Checklist — Inctagram/Somegram API (v1)

Цель — базовые “красные флаги” для API.

## Auth / Session
- [ ] Private endpoints без токена → 401 (не 200)
- [ ] С невалидным токеном → 401
- [ ] Logout инвалидирует доступ (нет доступа к /users/profile после logout)
- [ ] Refresh-token не даёт “вечные” токены без ротации (если заявлена ротация)

## Input validation
- [ ] Слишком длинные строки → 400/413 (не 500)
- [ ] Некорректные типы (number вместо string) → 400

## Authorization (IDOR smoke)
- [ ] Нельзя удалить/изменить чужой пост (403/404, но не 200)
- [ ] Нельзя смотреть приватные данные другого пользователя без прав

## Rate limiting / abuse
- [ ] Многократные login/recovery не ломают сервис

## Sensitive data
- [ ] В ответах нет паролей/секретов
- [ ] Ошибки не раскрывают stacktrace
