# PUBLIC USERS — API Test Cases

Версия: 1.0  
Окружение: Postman/Newman  
Модуль: Public Users

---

## TC-PUBU-001
Title: Get total count registered users (public)  
US: US-PUBLIC-USER-1  
Module: Public Users  
Type: Functional / Contract  
Priority: P3  
Severity: Trivial  
Endpoint: GET `/api/v1/public-users`

Preconditions: —
Test Data: —

Steps:
1) Открыть запрос **Get total count registered users in app**.
2) Отправить запрос без авторизации.
3) Зафиксировать статус и ответ.

Expected Result:
- 200/2xx.
- В ответе возвращается число/объект с total count (по контракту).
- Нет 500.

Postconditions: —
Attachments:
- Postman: `api/v1/public-users/...`

---

## TC-PUBU-002
Title: Get public profile info for user by id (public)  
US: US-PUBLIC-USER-2  
Module: Public Users  
Type: Functional / Contract  
Priority: P2  
Severity: Minor  
Endpoint: GET `/api/v1/public-users/profile/{userId}`

Preconditions:
- Известен валидный `userId`.

Test Data:
- userId: `<existing_user_id>`

Steps:
1) Открыть запрос **Get public profile info for user by id**.
2) Подставить `userId`.
3) Отправить запрос без авторизации.
4) Проверить статус и структуру публичного профиля.

Expected Result:
- 200/2xx (или 404, если userId не существует — по контракту).
- Ответ содержит только публичные поля (без приватных данных).

Postconditions: —
Attachments:
- Postman: `api/v1/public-users/profile/{userId}/...`
