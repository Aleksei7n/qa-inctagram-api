# API Test Cases — Inctagram

Версия: 1.0  
Окружение по умолчанию: Postman / Newman, `{{baseUrl}}`  
Авторизация: `{{bearerToken}}` (Bearer), refreshToken — cookie  
Источник эндпоинтов: Postman collection `api` (57 requests)

## Стандарт оформления
Порядок секций внутри тест-кейса:
1) Метаданные (US/Module/Type/Priority/Severity + Endpoint/Method)  
2) Preconditions  
3) Test Data  
4) Steps  
5) Expected Result  
6) Postconditions  
7) Attachments  

## Suites (наборы тестов)

### 1) Smoke Suite — Auth
Минимальный набор для проверки работоспособности авторизации.
- TC-AUTH-001 (registration)
- TC-AUTH-002 (registration confirmation)
- TC-AUTH-009 (login)
- TC-AUTH-011 (refresh-token)
- TC-AUTH-010 (logout)
- TC-AUTH-014 (me)

### 2) Smoke Suite — Core (Profile + Posts)
- TC-USER-001 (profile-info)
- TC-USER-003 (fill profile)
- TC-POST-009 (create post)
- TC-POST-001 (update post)
- TC-POST-002 (delete post)

### 3) Smoke Suite — Notifications + WSS
- TC-NOTIF-001 (get notifications)
- TC-WSS-001 (connect/listen)

---

### 4) Regression Suite — Full (по коллекции)
- Auth: TC-AUTH-001 … TC-AUTH-014
- Devices: TC-DEV-001 … TC-DEV-003
- Notifications: TC-NOTIF-001 … TC-NOTIF-003
- Users: TC-USER-001 … TC-USER-011
- Public Users: TC-PUBU-001 … TC-PUBU-002
- Posts: TC-POST-001 … TC-POST-009
- Public Posts: TC-PUBP-001 … TC-PUBP-003
- Following posts: TC-FOLL-001
- Country catalog: TC-LOC-001 … TC-LOC-002
- Subscriptions: TC-SUB-001 … TC-SUB-007
- WSS: TC-WSS-001 … TC-WSS-002

---

## Files (разделение по модулям)
- `auth.md` — TC-AUTH-001 … TC-AUTH-014  
- `devices.md` — TC-DEV-001 … TC-DEV-003  
- `notifications.md` — TC-NOTIF-001 … TC-NOTIF-003  
- `users.md` — TC-USER-001 … TC-USER-011  
- `public-users.md` — TC-PUBU-001 … TC-PUBU-002  
- `posts.md` — TC-POST-001 … TC-POST-009  
- `public-posts.md` — TC-PUBP-001 … TC-PUBP-003  
- `following.md` — TC-FOLL-001  
- `country-catalog.md` — TC-LOC-001 … TC-LOC-002  
- `subscriptions.md` — TC-SUB-001 … TC-SUB-007  
- `wss.md` — TC-WSS-001 … TC-WSS-002  
