# PUBLIC POSTS — API Test Cases

Версия: 1.0  
Окружение: Postman/Newman  
Модуль: Public Posts

---

## TC-PUBP-001
Title: Get public posts (public feed)  
US: US-PUBLIC-POST-1  
Module: Public Posts  
Type: Functional / Contract  
Priority: P2  
Severity: Minor  
Endpoint: GET `/api/v1/public-posts/all/{endCursorPostId}?pageSize=...`

Preconditions:
- Известны корректные параметры для фида (cursor/pageSize).

Test Data:
- endCursorPostId: `<cursor_or_0>`
- pageSize: `10`
- sortBy: `createdAt`
- sortDirection: `asc`

Steps:
1) Открыть запрос **Get public posts**.
2) Подставить cursor и query params.
3) Отправить запрос без авторизации.
4) Проверить структуру выдачи.

Expected Result:
- 200/2xx.
- Возвращается список публичных постов + мета, структура по контракту.

Postconditions:
- Сохранить `postId` для TC-PUBP-002/003.

Attachments:
- Postman: `api/v1/public-posts/all/{endCursorPostId}/...`

---

## TC-PUBP-002
Title: Get public post by id (positive)  
US: US-PUBLIC-POST-2  
Module: Public Posts  
Type: Functional / Contract  
Priority: P2  
Severity: Minor  
Endpoint: GET `/api/v1/public-posts/{postId}`

Preconditions:
- Есть валидный `postId` из TC-PUBP-001.

Test Data:
- postId: `<post_id>`

Steps:
1) Открыть запрос **Get public post by id**.
2) Подставить `postId`.
3) Отправить запрос.
4) Проверить статус и ключевые поля поста.

Expected Result:
- 200/2xx (или 404 если postId не существует — по контракту).
- Ответ соответствует контракту публичного поста.

Postconditions: —
Attachments:
- Postman: `api/v1/public-posts/{postId}/...`

---

## TC-PUBP-003
Title: Get comments for post by post id (public)  
US: US-PUBLIC-POST-3  
Module: Public Posts  
Type: Functional / Contract  
Priority: P3  
Severity: Trivial  
Endpoint: GET `/api/v1/public-posts/comments/{postId}?pageSize=...`

Preconditions:
- Есть `postId`.

Test Data:
- postId: `<post_id>`
- pageSize: `10`
- sortBy: `createdAt`
- sortDirection: `asc`

Steps:
1) Открыть запрос **Get comments for post by post id**.
2) Подставить `postId` и query params.
3) Отправить запрос без авторизации.
4) Проверить список комментариев и структуру элементов.

Expected Result:
- 200/2xx.
- Возвращается список комментариев (может быть пустым), структура по контракту.

Postconditions: —
Attachments:
- Postman: `api/v1/public-posts/comments/{postId}/...`
