# USERS (private) — API Test Cases

Версия: 1.0  
Окружение: Postman/Newman  
Модуль: Users

---

## TC-USER-001
Title: Get Profile info (authorized)  
US: US-USER-1  
Module: Users  
Type: Functional / Security-sanity / Contract  
Priority: P1  
Severity: Major  
Endpoint: GET `/api/v1/users/profile-info`

Preconditions:
- Есть `{{bearerToken}}`.

Test Data: —
Steps:
1) Открыть запрос **Get Profile info**.
2) Отправить запрос с Bearer token.
3) Проверить status code.
4) Проверить ключевые поля профиля (id/userName/email и т.п. — по контракту).

Expected Result:
- 200.
- Структура профиля соответствует контракту.

Postconditions: —

Attachments:
- Postman: `api/v1/users/profile-info/...`

---

## TC-USER-002
Title: Upload User Avatar (positive) — загрузка изображения  
US: US-USER-2  
Module: Users  
Type: Functional / Validation  
Priority: P2  
Severity: Minor  
Endpoint: POST `/api/v1/users/profile-upload-avatar`

Preconditions:
- Есть `{{bearerToken}}`.
- Подготовлен файл изображения (jpg/png).

Test Data:
- file: `avatar.jpg` (валидный формат, размер < 10 Мб)

Steps:
1) Открыть запрос **Upload User Avatar**.
2) Убедиться, что тип запроса `multipart/form-data`.
3) Вложить файл `avatar.jpg` в нужное поле формы.
4) Отправить запрос.
5) Проверить статус и ответ (url/флаг обновления — по контракту).
6) Выполнить TC-USER-001 и проверить, что avatarUrl/поле аватара обновилось (если предусмотрено).

Expected Result:
- 200 по контракту.
- Аватар загружен, профиль отражает изменения.

Postconditions:
- У пользователя установлен новый аватар.

Attachments:
- Postman: `api/v1/users/profile-upload-avatar/...`

---

## TC-USER-003
Title: Fill User Profile (positive) — заполнение профиля  
US: US-USER-3  
Module: Users  
Type: Functional / Contract  
Priority: P1  
Severity: Major  
Endpoint: PUT `/api/v1/users/profile-fill-info`

Preconditions:
- Есть `{{bearerToken}}`.

Test Data:
- firstName: `Ivan`
- lastName: `Ivanov`
- aboutMe: `Test bio`
- (другие поля — по контракту/коллекции)

Steps:
1) Открыть запрос **Fill User Profile**.
2) В Body указать валидные данные профиля.
3) Отправить запрос.
4) Проверить статус и ответ.
5) Выполнить TC-USER-001 и убедиться, что изменения применились.

Expected Result:
- 200.
- Профиль обновлён: значения в ответе/в `profile-info` соответствуют отправленным.

Postconditions:
- Профиль пользователя изменён.

Attachments:
- Postman: `api/v1/users/profile-fill-info/...`

---

## TC-USER-004
Title: Delete user avatar (positive)  
US: US-USER-4  
Module: Users  
Type: Functional  
Priority: P3  
Severity: Minor  
Endpoint: DELETE `/api/v1/users/profile-delete-avatar`

Preconditions:
- У пользователя есть загруженный аватар (TC-USER-002 выполнен).

Test Data: —
Steps:
1) Открыть запрос **Delete user avatar**.
2) Отправить запрос с Bearer token.
3) Проверить статус.
4) Выполнить TC-USER-001 и проверить, что avatarUrl очищен/удалён (по контракту).

Expected Result:
- 200.
- Аватар удалён, профиль отражает отсутствие аватара.

Postconditions:
- Аватар отсутствует.

Attachments:
- Postman: `api/v1/users/profile-delete-avatar/...`

---

## TC-USER-005
Title: Search users (authorized) — поиск пользователей  
US: US-USER-5  
Module: Users  
Type: Functional / Contract  
Priority: P2  
Severity: Minor  
Endpoint: GET `/api/v1/users/{endCursorUserId}?search=...&pageSize=...&pageNumber=...`

Preconditions:
- Есть `{{bearerToken}}`.

Test Data:
- search: `a`
- pageSize: `10`
- pageNumber: `1`
- endCursorUserId: `<start_cursor_or_0>` (по контракту)

Steps:
1) Открыть запрос **Search users**.
2) Заполнить query params: search/pageSize/pageNumber.
3) Отправить запрос.
4) Проверить статус.
5) Проверить, что ответ содержит список пользователей и/или пагинацию (по контракту).

Expected Result:
- 200.
- Ответ содержит массив пользователей (может быть пустым) и мета-данные пагинации.

Postconditions: —

Attachments:
- Postman: `api/v1/users/{endCursorUserId}/Search users`

---

## TC-USER-006
Title: Follow to user (positive)  
US: US-USER-6  
Module: Users  
Type: Functional  
Priority: P2  
Severity: Minor  
Endpoint: POST `/api/v1/users/follow/{followeeId}`

Preconditions:
- Есть `{{bearerToken}}`.
- Есть валидный `followeeId` другого пользователя.

Test Data:
- followeeId: `<target_user_id>`

Steps:
1) Открыть запрос **Follow to user**.
2) Подставить `followeeId`.
3) Отправить запрос.
4) Проверить статус.
5) Выполнить TC-USER-011 (Get following by user) и убедиться, что пользователь появился в following.

Expected Result:
- 200 по контракту.
- Подписка создаётся.

Postconditions:
- Текущий пользователь подписан на followeeId.

Attachments:
- Postman: `api/v1/users/follow/{followeeId}/...`

---

## TC-USER-007
Title: Unfollow to user (positive)  
US: US-USER-7  
Module: Users  
Type: Functional  
Priority: P2  
Severity: Minor  
Endpoint: POST `/api/v1/users/unfollow/{followeeId}`

Preconditions:
- Выполнен TC-USER-006 (есть подписка).

Test Data:
- followeeId: `<target_user_id>`

Steps:
1) Открыть запрос **Unollow to user**.
2) Подставить `followeeId`.
3) Отправить запрос.
4) Проверить статус.
5) Проверить списки following (TC-USER-011), что подписка удалена.

Expected Result:
- 200/204/2xx.
- Подписка удалена (по контракту).

Postconditions:
- Текущий пользователь не подписан на followeeId.

Attachments:
- Postman: `api/v1/users/unfollow/{followeeId}/...`

---

## TC-USER-008
Title: Delete follower (positive) — удалить подписчика  
US: US-USER-8  
Module: Users  
Type: Functional  
Priority: P3  
Severity: Minor  
Endpoint: DELETE `/api/v1/users/unfollow/{followerId}`

Preconditions:
- Есть `followerId` (кто подписан на текущего пользователя).

Test Data:
- followerId: `<follower_user_id>`

Steps:
1) Открыть запрос **Delete follower**.
2) Подставить `followerId`.
3) Отправить запрос.
4) Проверить статус.
5) Выполнить TC-USER-010 (followers list) и убедиться, что follower удалён.

Expected Result:
- 200.
- Подписчик удалён из followers (по контракту).

Postconditions: —

Attachments:
- Postman: `api/v1/users/unfollow/{followerId}/Delete follower`

---

## TC-USER-009
Title: Get profile info by userId (authorized)  
US: US-USER-9  
Module: Users  
Type: Functional / Contract  
Priority: P2  
Severity: Minor  
Endpoint: GET `/api/v1/users/{userId}/profile`

Preconditions:
- Есть `{{bearerToken}}`.
- Есть валидный `userId`.

Test Data:
- userId: `<existing_user_id>`

Steps:
1) Открыть запрос **Get profile info with posts count...**
2) Подставить `userId`.
3) Отправить запрос.
4) Проверить статус и ключевые поля: counts/профиль (по контракту).

Expected Result:
- 200 (или 404 для несуществующего id — по контракту).
- Структура профиля соответствует контракту.

Postconditions: —

Attachments:
- Postman: `api/v1/users/{userId}/profile/...`

---

## TC-USER-010
Title: Get followers by user (authorized) — список подписчиков  
US: US-USER-10  
Module: Users  
Type: Functional / Contract  
Priority: P3  
Severity: Minor  
Endpoint: GET `/api/v1/users/followers/{userId}/{endCursorUserId}?...`

Preconditions:
- Есть `{{bearerToken}}`.

Test Data:
- userId: `<existing_user_id>`
- endCursorUserId: `<cursor_or_0>`
- pageSize: `10`
- pageNumber: `1`
- search: `""` (или `a`)

Steps:
1) Открыть запрос **Get followers by user**.
2) Подставить path params userId/endCursorUserId и query params.
3) Отправить запрос.
4) Проверить статус.
5) Проверить массив followers и пагинацию.

Expected Result:
- 200.
- Ответ содержит список followers (может быть пустым) и мета-данные (по контракту).

Postconditions: —

Attachments:
- Postman: `api/v1/users/followers/{userId}/{endCursorUserId}/...`

---

## TC-USER-011
Title: Get following by user (authorized) — список подписок  
US: US-USER-11  
Module: Users  
Type: Functional / Contract  
Priority: P3  
Severity: Minor  
Endpoint: GET `/api/v1/users/following/{userId}/{endCursorUserId}?...`

Preconditions:
- Есть `{{bearerToken}}`.

Test Data:
- userId: `<existing_user_id>`
- endCursorUserId: `<cursor_or_0>`
- pageSize: `10`
- pageNumber: `1`
- search: `""`

Steps:
1) Открыть запрос **Get following by user**.
2) Подставить параметры.
3) Отправить запрос.
4) Проверить статус и структуру массива.

Expected Result:
- 200.
- Возвращается список following (может быть пустым), структура соответствует контракту.

Postconditions: —

Attachments:
- Postman: `api/v1/users/following/{userId}/{endCursorUserId}/...`
