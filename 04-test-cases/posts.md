# POSTS (private) — API Test Cases

Версия: 1.0  
Окружение: Postman/Newman  
Модуль: Posts + Comments

---

## TC-POST-001
Title: Update user post by id (positive)  
US: US-POST-UPDATE-1  
Module: Posts  
Type: Functional / Contract  
Priority: P2  
Severity: Minor  
Endpoint: PUT `/api/v1/posts/{postId}`

Preconditions:
- Есть `{{bearerToken}}`.
- Есть postId, принадлежащий текущему пользователю (создать через TC-POST-009 или взять существующий).

Test Data:
- postId: `<my_post_id>`
- description: `Updated description`

Steps:
1) Открыть запрос **Update user post by id**.
2) Подставить `postId`.
3) В Body изменить описание/поле согласно контракту.
4) Отправить запрос.
5) Проверить статус и ответ.
6) Выполнить TC-PUBP-002 и проверить изменения.

Expected Result:
- 200/2xx.
- В ответе обновлённые поля соответствуют отправленным.

Postconditions:
- Пост обновлён.

Attachments:
- Postman: `api/v1/posts/{postId}/Update user post by id`

---

## TC-POST-002
Title: Delete user post by id (positive)  
US: US-POST-DELETE-1  
Module: Posts  
Type: Functional  
Priority: P2  
Severity: Minor  
Endpoint: DELETE `/api/v1/posts/{postId}`

Preconditions:
- Есть `{{bearerToken}}`.
- Есть postId текущего пользователя.

Test Data:
- postId: `<my_post_id>`

Steps:
1) Открыть запрос **Delete user post by id**.
2) Подставить `postId`.
3) Отправить запрос.
4) Зафиксировать статус.
5) Повторить удаление того же postId — убедиться, что ответ предсказуем (404/ошибка по контракту, но не 500).

Expected Result:
- 200/204/2xx при первом удалении (по контракту).
- Повторное удаление возвращает предсказуемый код (404), без 500.

Postconditions:
- Пост удалён.

Attachments:
- Postman: `api/v1/posts/{postId}/Delete user post by id`

---

## TC-POST-003
Title: Get user posts (authorized) — получение постов пользователя  
US: US-POST-GET-1  
Module: Posts  
Type: Functional / Contract  
Priority: P3  
Severity: Minor  
Endpoint: GET `/api/v1/posts/{userId}/{endCursorPostId}?pageSize=...`

Preconditions:
- Есть `{{bearerToken}}`.
- Есть userId.

Test Data:
- userId: `<existing_user_id>`
- endCursorPostId: `<cursor_or_0>`
- pageSize: `10`
- sortBy: `createdAt` (пример)
- sortDirection: `asc` (пример)

Steps:
1) Открыть запрос **Get user posts**.
2) Подставить path/query params согласно контракту.
3) Отправить запрос.
4) Проверить статус и структуру выдачи (массив + пагинация, если есть).

Expected Result:
- 200/2xx.
- Ответ содержит список постов и мета-данные (по контракту).

Postconditions: —
Attachments:
- Postman: `api/v1/posts/{userId}/{endCursorPostId}/Get user posts`

---

## TC-POST-004
Title: Add like/unlike for post (smoke)  
US: US-POST-LIKE-1  
Module: Posts  
Type: Functional  
Priority: P3  
Severity: Trivial  
Endpoint: PUT `/api/v1/posts/like/{postId}`

Preconditions:
- Есть `{{bearerToken}}`.
- Есть валидный `postId`.

Test Data:
- postId: `<post_id>`

Steps:
1) Открыть запрос **Add like/unlike for post**.
2) Подставить `postId`.
3) Отправить запрос (1 раз) — зафиксировать статус.
4) Отправить повторно (2 раз) — проверить идемпотентность/переключение по контракту.

Expected Result:
- Ответ предсказуем: 200/204/2xx (по контракту).
- Повторная операция не приводит к 500.

Postconditions:
- Состояние лайка изменено согласно контракту.

Attachments:
- Postman: `api/v1/posts/like/{postId}/...`

---

## TC-POST-005
Title: Add comment for post (positive)  
US: US-COMMENT-ADD-1  
Module: Comments  
Type: Functional / Validation  
Priority: P2  
Severity: Minor  
Endpoint: POST `/api/v1/posts/comments/{postId}`

Preconditions:
- Есть `{{bearerToken}}`.
- Есть postId.

Test Data:
- postId: `<post_id>`
- content: `Nice post!`

Steps:
1) Открыть запрос **Add comment for post by current user**.
2) Подставить `postId`.
3) В Body указать текст комментария согласно контракту.
4) Отправить запрос.
5) Проверить статус и наличие commentId (если возвращается).

Expected Result:
- 200/201/2xx.
- Комментарий создан; возвращается commentId/данные комментария (по контракту).

Postconditions:
- Есть новый комментарий (commentId использовать в TC-POST-006/007/008).

Attachments:
- Postman: `api/v1/posts/comments/{postId}/...`

---

## TC-POST-006
Title: Add answer for comment (positive)  
US: US-COMMENT-ANSWER-1  
Module: Comments  
Type: Functional  
Priority: P3  
Severity: Trivial  
Endpoint: POST `/api/v1/posts/comments/answer-comment/{commentId}`

Preconditions:
- Есть `{{bearerToken}}`.
- Есть `commentId` (из TC-POST-005).

Test Data:
- commentId: `<comment_id>`
- content: `Thanks!`

Steps:
1) Открыть запрос **Add answer for comment**.
2) Подставить `commentId`.
3) Указать текст ответа.
4) Отправить запрос.
5) Зафиксировать статус и id ответа.

Expected Result:
- 200/201/2xx.
- Ответ на комментарий создан (по контракту).

Postconditions:
- Создан answer-comment.

Attachments:
- Postman: `api/v1/posts/comments/answer-comment/{commentId}/...`

---

## TC-POST-007
Title: Add like/dislike for comment (smoke)  
US: US-COMMENT-LIKE-1  
Module: Comments  
Type: Functional  
Priority: P3  
Severity: Trivial  
Endpoint: PUT `/api/v1/posts/comments/like/{commentId}`

Preconditions:
- Есть `{{bearerToken}}`.
- Есть `commentId`.

Test Data:
- commentId: `<comment_id>`

Steps:
1) Открыть запрос **Add like/dislike for comment**.
2) Подставить `commentId`.
3) Отправить запрос 1 раз.
4) Отправить запрос 2 раз (проверка переключения/идемпотентности по контракту).

Expected Result:
- 200/204/2xx.
- Повтор не приводит к 500; поведение предсказуем.

Postconditions: —
Attachments:
- Postman: `api/v1/posts/comments/like/{commentId}/...`

---

## TC-POST-008
Title: Get comment answers by comment id (positive)  
US: US-COMMENT-GET-ANSWERS-1  
Module: Comments  
Type: Functional / Contract  
Priority: P3  
Severity: Trivial  
Endpoint: GET `/api/v1/posts/comments/{commentId}/answer-comment?...`

Preconditions:
- Есть `{{bearerToken}}`.
- Есть `commentId` с ответами (TC-POST-006 выполнен).

Test Data:
- commentId: `<comment_id>`
- pageSize: `10`
- sortBy: `createdAt`
- sortDirection: `asc`

Steps:
1) Открыть запрос **Get comment answers by comment id**.
2) Подставить `commentId` и query params.
3) Отправить запрос.
4) Проверить структуру выдачи ответов.

Expected Result:
- 200/2xx.
- Возвращается список answer-comments (включая созданный ранее), структура по контракту.

Postconditions: —
Attachments:
- Postman: `api/v1/posts/comments/{commentId}/answer-comment/...`

---

## TC-POST-009
Title: Add user post (positive) — создание поста  
US: US-POST-CREATE-1  
Module: Posts  
Type: Functional / Contract  
Priority: P1  
Severity: Major  
Endpoint: POST `/api/v1/posts`

Preconditions:
- Есть `{{bearerToken}}`.

Test Data:
- description: `My first post`
- фото (размер < 10 Мб)

Steps:
1) Открыть запрос **Add user post**.
2) Заполнить Body согласно контракту (обязательные поля).
3) Отправить запрос.
4) Проверить статус и что возвращается `postId`/идентификатор.
5) Сохранить `postId` для TC-POST-001/002.

Expected Result:
- 200/201/2xx.
- Пост создан, возвращён postId/данные поста (по контракту).

Postconditions:
- Есть новый пост (postId сохранён).

Attachments:
- Postman: `api/v1/posts/Add user post`
