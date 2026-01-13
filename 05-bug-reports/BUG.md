# BUG-001

**ID:** BUG-001  
**Title:** Logout does not invalidate accessToken: private endpoint still доступен после logout  
**Project:** Inctagram API (v1)  
**Component/Module:** Auth / Logout  
**Environment:** Postman (Desktop), baseUrl={{baseUrl}}  
**URL/Page:** `POST /api/v1/auth/logout`, `GET /api/v1/users/profile`  
**Build/Date:** YYYY-MM-DD  
**Severity:** Critical  
**Priority:** P0  
**Reproducibility:** Always  

## Preconditions
- Есть валидный пользователь.
- Выполнен login, переменная `{{bearerToken}}` установлена (валидный accessToken).

## Test Data
- `{{bearerToken}}`: valid accessToken

## Steps to reproduce
1. Выполнить запрос **POST** `/api/v1/auth/logout` с заголовком `Authorization: Bearer {{bearerToken}}`.
2. Убедиться, что запрос logout завершился успешно (ожидаемый статус — по контракту **204**).
3. Не изменяя значение `{{bearerToken}}`, выполнить запрос **GET** `/api/v1/users/profile`.
4. Зафиксировать status code и response body.

## Actual result
- `GET /api/v1/users/profile` возвращает **200 OK** и данные профиля, как будто токен остаётся валидным.

## Expected result
- После успешного logout приватные эндпоинты должны быть недоступны с предыдущим accessToken:
  - `GET /api/v1/users/profile` возвращает **401 Unauthorized** (или иной предусмотренный контрактом результат, но не 200).
- AccessToken должен считаться недействительным после logout (для предотвращения повторного использования).

## Attachments
- Postman evidence (Examples / saved responses):
  - `08-postman/evidence/BUG-001_logout_response.json`
  - `08-postman/evidence/BUG-001_profile_after_logout_response.json`

---

# BUG-002

**ID:** BUG-002  
**Title:** Registration: invalid email format returns 500 instead of 400 validation error  
**Project:** Inctagram API (v1)  
**Component/Module:** Auth / Registration  
**Environment:** Postman (Desktop), baseUrl={{baseUrl}}  
**URL/Page:** `POST /api/v1/auth/registration`  
**Build/Date:** YYYY-MM-DD  
**Severity:** Major  
**Priority:** P1  
**Reproducibility:** Always  

## Preconditions
- Нет.

## Test Data
- email: `test@` (невалидный формат)
- username: `qa_user_123`
- password: `Qwerty123!`

## Steps to reproduce
1. Выполнить запрос **POST** `/api/v1/auth/registration`.
2. Передать body с `email="test@"`, остальные поля оставить валидными.
3. Зафиксировать status code и body ответа.

## Actual result
- API возвращает **500 Internal Server Error**.

## Expected result
- API должен возвращать **400 Bad Request** (validation error) с понятным описанием:
  - поле `email` некорректно,
  - структура ошибки соответствует контракту (единый формат ошибок, без 500).

## Attachments
- Postman evidence (Examples / saved responses):
  - `08-postman/evidence/BUG-002_registration_invalid_email_request.json`
  - `08-postman/evidence/BUG-002_registration_invalid_email_response.json`

---

# BUG-003

**ID:** BUG-003  
**Title:** Posts: User B can update/delete post of User A (authorization/IDOR issue)  
**Project:** Inctagram API (v1)  
**Component/Module:** Posts / Authorization  
**Environment:** Postman (Desktop), baseUrl={{baseUrl}}  
**URL/Page:** `PUT /api/v1/posts/:id` и/или `DELETE /api/v1/posts/:id`  
**Build/Date:** YYYY-MM-DD  
**Severity:** Critical  
**Priority:** P0  
**Reproducibility:** Always  

## Preconditions
- Есть два пользователя: **User A** и **User B**.
- Под User A создан пост → известен `postId`.
- Выполнен login под User B → `{{bearerToken}}` принадлежит **User B**.

## Test Data
- postId: `<post_of_user_A>`
- Authorization: `Bearer {{bearerToken}}` (токен User B)

## Steps to reproduce
1. Под User B выполнить **DELETE** `/api/v1/posts/<postId_of_user_A>`  
   *(или **PUT** `/api/v1/posts/<postId_of_user_A>` с изменением данных поста).*
2. Зафиксировать status code.
3. (Для delete) Выполнить `GET /api/v1/posts/<postId_of_user_A>` или открыть пост через ленту, чтобы подтвердить факт удаления.  
   (Для update) Повторно получить пост и подтвердить факт изменения.

## Actual result
- API возвращает **200/204** и операция проходит успешно: чужой пост удаляется/обновляется.

## Expected result
- Операции над чужим ресурсом должны быть запрещены:
  - **403 Forbidden** (предпочтительно).
- Ресурс User A не должен изменяться/удаляться токеном User B.

## Attachments
- Postman evidence (Examples / saved responses):
  - `08-postman/evidence/BUG-003_delete_foreign_post_response.json`
  - `08-postman/evidence/BUG-003_get_post_after_delete_response.json`
