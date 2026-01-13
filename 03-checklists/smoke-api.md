# Smoke Checklist — Inctagram API (v1)

## Auth
- [ ] POST `/auth/registration` возвращает ожидаемый статус (201/204/200 — согласно контракту) / или валидную ошибку
- [ ] POST `/auth/login` → получаем `accessToken`
- [ ] POST `/auth/refresh-token` (с валидным refresh) → новый `accessToken`
- [ ] POST `/auth/logout` → 204

## Devices
- [ ] GET `/devices` → список сессий (200)
- [ ] DELETE `/devices/terminate/:deviceId` → 204
- [ ] DELETE `/devices/terminate-all` → 204

## Users/Profile
- [ ] GET `/users/profile` (с токеном) → 200
- [ ] PUT `/users/profile` → 200 + данные обновились
- [ ] PUT `/users/password` → 204/200 (согласно контракту)

## Posts
- [ ] POST `/posts` → пост создан (201/200)
- [ ] GET `/posts` → пост виден в списке/фиде
- [ ] PUT `/posts/:id` → обновление успешно
- [ ] DELETE `/posts/:id` → удаление успешно

## Public
- [ ] GET `/public-users/profile/:username` → 200/404 (по данным)
- [ ] GET `/public-posts` → 200 + массив/пагинация

## Subs / Catalog / Notifications
- [ ] GET `/country-catalog/countries` → 200
- [ ] GET `/notifications` (auth) → 200
- [ ] GET `/subscriptions/current-subscription` (auth) → 200
