# FOLLOWING POSTS — API Test Cases

Версия: 1.0  
Окружение: Postman/Newman  
Модуль: Following

---

## TC-FOLL-001
Title: Get following posts (authorized) — лента подписок  
US: US-FOLLOWING-POSTS-1  
Module: Following  
Type: Functional / Contract  
Priority: P3  
Severity: Minor  
Endpoint: GET `/api/v1/following/posts/{endCursorPostId}?pageSize=...`

Preconditions:
- Есть `{{bearerToken}}`.
- Текущий пользователь подписан хотя бы на одного пользователя (выполнить TC-USER-006).

Test Data:
- endCursorPostId: `<cursor_or_0>`
- pageSize: `10`
- sortBy: `createdAt`
- sortDirection: `asc`

Steps:
1) Открыть запрос **Get following posts**.
2) Подставить cursor и query params.
3) Отправить запрос с Bearer token.
4) Проверить статус и структуру ленты.

Expected Result:
- 200.
- Возвращается список постов подписок (может быть пустым), структура по контракту.

Postconditions: —

Attachments:
- Postman: `api/v1/following/posts/{endCursorPostId}/...`
