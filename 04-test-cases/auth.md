# AUTH — API Test Cases

Версия: 1.0  
Окружение по умолчанию: Postman/Newman, `{{baseUrl}}`  
Переменные: `{{baseUrl}}`, `{{bearerToken}}`  
Модуль: Auth

---

## TC-AUTH-001
Title: Registration (positive) — создание пользователя  
US: US-AUTH-REG-1  
Module: Auth  
Type: Functional / Contract  
Priority: P1  
Severity: Major  
Endpoint: POST `/api/v1/auth/registration`

Preconditions:
- В Postman установлен `{{baseUrl}}`.
- Email/username не заняты (уникальные данные).

Test Data:
- email: `new_user_<timestamp>@mail.com`
- userName: `new_user_<timestamp>`
- password: `Qwerty123!`

Steps:
1) Открыть запрос **User registration** в коллекции.
2) Убедиться, что выбран метод `POST` и URL `{{baseUrl}}/api/v1/auth/registration`.
3) В Body указать валидные данные (уникальные email/userName).
4) Отправить запрос.
5) Зафиксировать status code и response body.

Expected Result:
- Возвращается **2xx**.
- Response не содержит технических ошибок (stack trace/500).
- В ответе есть confirmation (по контракту).

Postconditions:
- Пользователь создан в системе (в статусе “не подтверждён”).

Attachments:
- Postman: `api/v1/auth/registration/User registration` (request + response)
- (optional) `assets/postman-snapshots/TC-AUTH-001.json`

---

## TC-AUTH-002
Title: Registration confirmation (positive) — подтверждение регистрации  
US: US-AUTH-REG-2  
Module: Auth  
Type: Functional  
Priority: P1  
Severity: Major  
Endpoint: POST `/api/v1/auth/registration-confirmation`

Preconditions:
- Выполнен TC-AUTH-001.
- Есть `confirmationCode`/token (из письма/логов/тестового ответа стенда).

Test Data:
- confirmationCode: `<valid_confirmation_code>`

Steps:
1) Открыть запрос **Registration Confirmation**.
2) Указать валидный `confirmationCode` согласно контракту.
3) Отправить запрос.
4) Зафиксировать статус ответа.

Expected Result:
- Возвращается **2xx** (200 по контракту).
- Пользователь считается подтверждённым (подтверждается дальнейшим успешным логином TC-AUTH-009).

Postconditions:
- Аккаунт активен и может логиниться.

Attachments:
- Postman: `api/v1/auth/registration-confirmation/Registration Confirmation`

---

## TC-AUTH-003
Title: Registration email resending (positive) — повторная отправка письма подтверждения  
US: US-AUTH-REG-3  
Module: Auth  
Type: Functional  
Priority: P2  
Severity: Minor  
Endpoint: POST `/api/v1/auth/registration-email-resending`

Preconditions:
- Пользователь существует и НЕ подтверждён.
- Почтовый сервис на стенде может быть отключён (учесть риск).

Test Data:
- email: `new_user_<timestamp>@mail.com`

Steps:
1) Открыть запрос **Resend confirmation registration Email if user exists**.
2) В Body указать email существующего непотверждённого пользователя.
3) Отправить запрос.
4) Зафиксировать статус и тело ответа.

Expected Result:
- Возвращается **2xx**.
- Не происходит утечки информации (по контракту может быть одинаковый ответ для существующего/несуществующего email).
- Нет 500.

Postconditions:
- Пользователь получает новый confirmation (если инфраструктура включена).

Attachments:
- Postman: `api/v1/auth/registration-email-resending/...`

---

## TC-AUTH-004
Title: Google Auth (smoke) — старт OAuth-флоу  
US: US-AUTH-OAUTH-1  
Module: Auth  
Type: Functional / Integration-sanity  
Priority: P3  
Severity: Minor  
Endpoint: GET `/api/v1/auth/google`

Preconditions:
- Стенд доступен из сети.
- OAuth может быть отключён на тестовом стенде.

Test Data: —
Steps:
1) Открыть запрос **Auth Controller google Auth**.
2) Отправить GET запрос.
3) Зафиксировать поведение: redirect/ошибка по контракту.

Expected Result:
- Возвращается redirect (3xx) на провайдера **или** ошибка по контракту.
- Нет 500/stack trace.

Postconditions: —

Attachments:
- Postman: `api/v1/auth/google/...`

---

## TC-AUTH-005
Title: Google callback (smoke) — обработка возврата от провайдера  
US: US-AUTH-OAUTH-2  
Module: Auth  
Type: Functional / Integration-sanity  
Priority: P3  
Severity: Minor  
Endpoint: GET `/api/v1/auth/google/callback`

Preconditions:
- Без валидных query-параметров может вернуться ошибка.

Test Data:
- Query params: `code=<invalid_or_empty>`

Steps:
1) Открыть запрос **Google Authentication Callback**.
2) Выполнить запрос без параметров либо с тестовыми параметрами.
3) Зафиксировать статус и ответ.

Expected Result:
- Ответ предсказуем по контракту (ошибка валидации/redirect), но **не 500**.

Postconditions: —

Attachments:
- Postman: `api/v1/auth/google/callback/...`

---

## TC-AUTH-006
Title: Get reCAPTCHA site key — получение публичного ключа  
US: US-AUTH-SEC-1  
Module: Auth  
Type: Functional / Contract  
Priority: P3  
Severity: Minor  
Endpoint: GET `/api/v1/auth/recaptcha-site-key`

Preconditions: —
Test Data: —

Steps:
1) Открыть запрос **Get the reCAPTCHA site key**.
2) Отправить GET запрос.
3) Проверить структуру ответа.

Expected Result:
- Возвращается 200.
- В ответе присутствует ключ/значение siteKey (точное поле — по контракту).
- Нет чувствительных данных (private keys).

Postconditions: —

Attachments:
- Postman: `api/v1/auth/recaptcha-site-key/...`

---

## TC-AUTH-007
Title: Restore password (positive) — инициировать восстановление  
US: US-AUTH-PASS-1  
Module: Auth  
Type: Functional  
Priority: P1  
Severity: Major  
Endpoint: POST `/api/v1/auth/restore-password`

Preconditions:
- Пользователь с email существует.

Test Data:
- email: `existing_user@mail.com`

Steps:
1) Открыть запрос **Restore Password**.
2) В Body указать существующий email.
3) Отправить запрос.
4) Зафиксировать ответ.

Expected Result:
- Возвращается **2xx**.
- Нет 500.
- Ответ не раскрывает лишнее.

Postconditions:
- Пользователь получает код/ссылку восстановления.

Attachments:
- Postman: `api/v1/auth/restore-password/...`

---

## TC-AUTH-008
Title: Restore password confirmation (positive) — подтверждение и установка нового пароля  
US: US-AUTH-PASS-2  
Module: Auth  
Type: Functional  
Priority: P1  
Severity: Major  
Endpoint: POST `/api/v1/auth/restore-password-confirmation`

Preconditions:
- Выполнен TC-AUTH-007.
- Есть `recoveryCode`/token.

Test Data:
- recoveryCode: `<valid_recovery_code>`
- newPassword: `NewPass123!`

Steps:
1) Открыть запрос **Restore Password Confirmation**.
2) В Body указать валидный recoveryCode и новый пароль.
3) Отправить запрос.
4) Зафиксировать статус.
5) Выполнить логин (TC-AUTH-009) с новым паролем для подтверждения.

Expected Result:
- Возвращается **2xx**.
- Логин с новым паролем успешен (в рамках подтверждения результата).

Postconditions:
- Пароль пользователя обновлён.

Attachments:
- Postman: `api/v1/auth/restore-password-confirmation/...`

---

## TC-AUTH-009
Title: Login (positive) — получение access token  
US: US-AUTH-LOGIN-1  
Module: Auth  
Type: Functional / Security-sanity  
Priority: P0  
Severity: Critical  
Endpoint: POST `/api/v1/auth/login`

Preconditions:
- Пользователь существует и подтверждён.

Test Data:
- email: `existing_user@mail.com`
- password: `ValidPass123!`

Steps:
1) Открыть запрос **User Login**.
2) В Body указать валидные креды.
3) Отправить запрос.
4) Проверить, что в ответе есть токен.
5) Сохранить accessToken в `{{bearerToken}}` (через Tests/Set variable или вручную).
6) Выполнить TC-AUTH-014 (me) как проверку токена.

Expected Result:
- Возвращается 200 по контракту.
- Возвращён accessToken (и refresh cookie).
- С токеном приватный эндпоинт `me` отдаёт 200.

Postconditions:
- `{{bearerToken}}` установлен для дальнейших запросов.

Attachments:
- Postman: `api/v1/auth/login/User Login`

---

## TC-AUTH-010
Title: Logout (positive) — отзыв refreshToken (если используется)  
US: US-AUTH-LOGOUT-1  
Module: Auth  
Type: Functional / Security-sanity  
Priority: P0  
Severity: Major  
Endpoint: POST `/api/v1/auth/logout`

Preconditions:
- Выполнен TC-AUTH-009.
- Клиент отправляет refreshToken cookie.

Test Data: —
Steps:
1) Открыть запрос **Logout current user...**.
2) Убедиться, что запрос отправляется с cookie (если требуется).
3) Отправить запрос.
4) Зафиксировать статус ответа.
5) Выполнить TC-AUTH-011 (refresh-token) — ожидаем отказ (если refresh отозван).

Expected Result:
- Возвращается 204 по контракту.
- После logout refresh-token не выдаёт новый accessToken (401/предсказуемый отказ).

Postconditions:
- Сессия завершена (по контракту).

Attachments:
- Postman: `api/v1/auth/logout/...`

---

## TC-AUTH-011
Title: Refresh-token (positive) — выдача нового access token  
US: US-AUTH-REFRESH-1  
Module: Auth  
Type: Functional / Security-sanity  
Priority: P0  
Severity: Critical  
Endpoint: POST `/api/v1/auth/refresh-token`

Preconditions:
- Есть валидный refreshToken cookie (обычно после TC-AUTH-009).
- Logout не выполнен (иначе это negative сценарий).

Test Data: —
Steps:
1) Открыть запрос **Generate new access token...**.
2) Отправить запрос с валидным refresh cookie.
3) Зафиксировать статус и наличие нового accessToken.
4) Обновить `{{bearerToken}}` новым токеном.
5) Выполнить TC-AUTH-014 (me).

Expected Result:
- Возвращается 200.
- Возвращается новый accessToken (по контракту).
- С новым токеном `me` отдаёт 200.

Postconditions:
- `{{bearerToken}}` обновлён.

Attachments:
- Postman: `api/v1/auth/refresh-token/...`

---

## TC-AUTH-012
Title: GitHub Auth (smoke) — старт OAuth-флоу  
US: US-AUTH-OAUTH-3  
Module: Auth  
Type: Functional / Integration-sanity  
Priority: P3  
Severity: Minor  
Endpoint: GET `/api/v1/auth/github`

Preconditions: —
Test Data: —

Steps:
1) Открыть запрос **Auth Controller github Auth**.
2) Выполнить запрос.
3) Зафиксировать redirect/ответ.

Expected Result:
- Redirect (3xx) или предсказуемая ошибка.
- Нет 500.

Postconditions: —

Attachments:
- Postman: `api/v1/auth/github/...`

---

## TC-AUTH-013
Title: GitHub callback (smoke) — обработка возврата  
US: US-AUTH-OAUTH-4  
Module: Auth  
Type: Functional / Integration-sanity  
Priority: P3  
Severity: Minor  
Endpoint: GET `/api/v1/auth/github/callback`

Preconditions: —
Test Data:
- Query params: `code=<invalid_or_empty>` (если возможно)

Steps:
1) Открыть запрос **GitHub Authentication Callback**.
2) Выполнить запрос без параметров/с тестовыми параметрами.
3) Зафиксировать статус.

Expected Result:
- Предсказуемый ответ по контракту, **не 500**.

Postconditions: —

Attachments:
- Postman: `api/v1/auth/github/callback/...`

---

## TC-AUTH-014
Title: Get info about user (me) — доступ по Bearer token  
US: US-AUTH-ME-1  
Module: Auth  
Type: Functional / Security-sanity / Contract  
Priority: P1  
Severity: Major  
Endpoint: GET `/api/v1/auth/me`

Preconditions:
- Выполнен TC-AUTH-009 и установлен `{{bearerToken}}`.

Test Data: —
Steps:
1) Открыть запрос **Get info about user**.
2) Убедиться, что Authorization: `Bearer {{bearerToken}}` присутствует (в Headers/Authorization).
3) Отправить запрос.
4) Проверить status code и структуру ответа (ключевые поля пользователя).

Expected Result:
- 200.
- В ответе возвращаются данные текущего пользователя (id/email/userName — по контракту).
- Нет лишних чувствительных данных (пароль/хэши/секреты).

Postconditions: —

Attachments:
- Postman: `api/v1/auth/me/...`
