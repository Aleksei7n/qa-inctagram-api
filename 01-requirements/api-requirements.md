# API Requirements & Endpoint Inventory — Inctagram

Источник: Postman collection “api swagger” (v1).  
Базовый URL задаётся переменной `{{baseUrl}}`. bearer-protected с `{{bearerToken}}`.  

---

## 1) High-level requirements (User Stories)

### US-AUTH-01 Registration
Как новый пользователь, я могу зарегистрироваться по email/username/password, чтобы создать аккаунт.

### US-AUTH-02 Registration confirmation
Как новый пользователь, я могу подтвердить регистрацию по токену/коду подтверждения.

### US-AUTH-03 Login
Как пользователь, я могу залогиниться и получить `accessToken` (и refresh token в cookie), чтобы выполнять авторизованные запросы.

### US-AUTH-04 Refresh token
Как пользователь, я могу обновить пару токенов, чтобы продолжать сессию без повторного логина.

### US-AUTH-05 Logout
Как пользователь, я могу завершить сессию, чтобы токены стали недействительными.

### US-AUTH-06 Password recovery & new password
Как пользователь, я могу запросить восстановление пароля и установить новый пароль.

### US-DEV-01 Devices sessions management
Как пользователь, я могу посмотреть список устройств/сессий и завершать сессии.

### US-USER-01 Profile management
Как пользователь, я могу получать/обновлять профиль, менять email/пароль, управлять аватаром.

### US-SOC-01 Follow management
Как пользователь, я могу подписываться/отписываться и видеть списки followers/following.

### US-POST-01 Posts
Как пользователь, я могу создавать/читать/редактировать/удалять посты, лайкать/дизлайкать.

### US-PUBLIC-01 Public access
Как гость, я могу просматривать публичные профили/посты.

### US-NOTIF-01 Notifications
Как пользователь, я могу получать список уведомлений.

### US-SUB-01 Subscriptions
Как пользователь, я могу создать/отменить подписку и посмотреть статус/платежи.

### US-LOC-01 Country catalog
Как пользователь, я могу получать стран/регионов/городов.

---

## 2) API inventory (v1)

Формат: **METHOD** `path` — purpose

### Auth
- **POST** `/api/v1/auth/registration` — register
- **POST** `/api/v1/auth/registration-confirmation` — confirm registration
- **POST** `/api/v1/auth/login` — login
- **POST** `/api/v1/auth/refresh-token` — refresh tokens
- **GET**  `/api/v1/auth/github/callback` — OAuth callback (redirect behavior)
- **POST** `/api/v1/auth/password-recovery` — start recovery
- **POST** `/api/v1/auth/new-password` — set new password
- **POST** `/api/v1/auth/logout` — logout (204 expected)

### Devices
- **GET**    `/api/v1/devices` — list sessions/devices
- **DELETE** `/api/v1/devices/terminate/:deviceId` — end session by ID
- **DELETE** `/api/v1/devices/terminate-all` — end all sessions

### Notifications
- **GET** `/api/v1/notifications` — notifications list

### Users (private)
- **GET**  `/api/v1/users/profile` — my profile
- **PUT**  `/api/v1/users/profile` — update my profile
- **POST** `/api/v1/users/profile/avatar` — upload avatar
- **DELETE** `/api/v1/users/profile/avatar` — delete avatar
- **PUT**  `/api/v1/users/email` — change email
- **POST** `/api/v1/users/email/confirmation` — confirm new email
- **PUT**  `/api/v1/users/password` — change password
- **GET**  `/api/v1/users/:id` — get user by id
- **GET**  `/api/v1/users/:id/followers` — followers by user id
- **GET**  `/api/v1/users/:id/following` — following by user id
- **GET**  `/api/v1/users/my-followers` — my followers
- **GET**  `/api/v1/users/my-following` — my following
- **PUT**  `/api/v1/users/:id/follow` — follow
- **PUT**  `/api/v1/users/:id/unfollow` — unfollow

### Public-users
- **GET** `/api/v1/public-users/profile/:username`
- **GET** `/api/v1/public-users/profile/:username/following`
- **GET** `/api/v1/public-users/profile/:username/followers`

### Posts (private)
- **POST** `/api/v1/posts` — create post
- **GET**  `/api/v1/posts` — list/feed
- **GET**  `/api/v1/posts/:id` — get by id
- **PUT**  `/api/v1/posts/:id` — update
- **DELETE** `/api/v1/posts/:id` — delete
- **PUT**  `/api/v1/posts/:id/like`
- **PUT**  `/api/v1/posts/:id/unlike`
- **GET**  `/api/v1/posts/user/:userId` — list by user

### Public-posts
- **GET** `/api/v1/public-posts` — public feed
- **GET** `/api/v1/public-posts/:postId`
- **GET** `/api/v1/public-posts/profile/:username`
- **GET** `/api/v1/public-posts/profile/:username/:postId`
- **GET** `/api/v1/public-posts/following` — public “following feed” (if supported)

### Following
- **POST**   `/api/v1/following/add`
- **GET**    `/api/v1/following/get`
- **DELETE** `/api/v1/following/delete/:userId`

### Country catalog
- **GET** `/api/v1/country-catalog/countries`
- **GET** `/api/v1/country-catalog/regions/:countryId`
- **GET** `/api/v1/country-catalog/cities/:countryId`

### Subscriptions
- **POST**   `/api/v1/subscriptions/create`
- **DELETE** `/api/v1/subscriptions/cancel`
- **GET**    `/api/v1/subscriptions/current-subscription`
- **GET**    `/api/v1/subscriptions/my-payments`

### WSS (availability / smoke)
- WebSocket endpoints из коллекции: check basic connectivity/handshake (smoke-only)

---

## 3) Common expectations (non-functional / contract)
- HTTP status codes are meaningful (200/201/204, 400, 401, 403, 404, 409, 429, 500)
- Error response format is stable (message, errors array — if exists)
- Response time sanity: основные read-эндпоинты до ~1–2s на стенде
- Auth: без токена приватные эндпоинты возвращают 401
- Logout/terminate: корректная инвалидация токенов/сессий
