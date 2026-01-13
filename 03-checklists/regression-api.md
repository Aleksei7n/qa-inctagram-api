# Regression Checklist — Inctagram/Somegram API (v1)

## Общие проверки (для каждого эндпоинта)
- [ ] Корректный HTTP status code (success / error)
- [ ] Content-Type соответствует (JSON там, где ожидается)
- [ ] Контракт: обязательные поля присутствуют, типы данных соответствуют
- [ ] Ошибки: формат ошибки стабилен (message / errors)
- [ ] Без токена (на private) → 401
- [ ] Некорректный параметр/ID → 400/404 (не 500)

## Auth
- [ ] Невалидный пароль → 401/400 (не 500)
- [ ] Refresh-token: старый refresh отзывается после refresh
- [ ] Logout: после logout приватные эндпоинты не доступны

## Users
- [ ] Update profile: валидации
- [ ] Change email: требует подтверждение (confirmation)
- [ ] Avatar upload: ограничения размера/формата
- [ ] Follow/unfollow: idempotency (повторное follow не ломает)

## Posts
- [ ] Create post: обязательные поля
- [ ] Read post: 404 на несуществующий ID
- [ ] Like/unlike: повторные операции предсказуемы
- [ ] Delete: после удаления GET по ID → 404

## Public
- [ ] Публичные ручки не требуют токен
- [ ] Параметры username корректно валидируются

## Subscriptions
- [ ] Create/cancel: корректные статусы, нет “залипания”
- [ ] current-subscription: отражает текущее состояние

## Country catalog
- [ ] countries/regions/cities: стабильная структура и сортировка
