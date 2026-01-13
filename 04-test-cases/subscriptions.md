# SUBSCRIPTIONS — API Test Cases

Версия: 1.0  
Окружение: Postman/Newman  
Модуль: Subscriptions

---

## TC-SUB-001
Title: Create payment-subscriptions (authorized)  
US: US-SUB-1  
Module: Subscriptions  
Type: Functional / Contract  
Priority: P1  
Severity: Major  
Endpoint: POST `/api/v1/subscriptions/create-payment`

Preconditions:
- Есть `{{bearerToken}}`.
- Подписки/платёжный провайдер на стенде может быть отключён.

Test Data:
- plan/type: `<value_by_contract>`
- currency: `<value_by_contract>`

Steps:
1) Открыть запрос **Create payment-subscriptions**.
2) Заполнить Body согласно контракту.
3) Отправить запрос.
4) Проверить статус и наличие paymentUrl/transactionId.

Expected Result:
- 200 (или предсказуемая заглушка на стенде).
- В ответе есть данные для оплаты/идентификаторы (по контракту).
- Нет 500.

Postconditions:
- Создан платёж/инициирована подписка (по контракту).

Attachments:
- Postman: `api/v1/subscriptions/create-payment/...`

---

## TC-SUB-002
Title: Get payments (authorized) — история платежей  
US: US-SUB-2  
Module: Subscriptions  
Type: Functional / Contract  
Priority: P2  
Severity: Minor  
Endpoint: GET `/api/v1/subscriptions/my-payments?pageSize=...&pageNumber=...`

Preconditions:
- Есть `{{bearerToken}}`.

Test Data:
- pageSize: `10`
- pageNumber: `1`

Steps:
1) Открыть запрос **Get payments**.
2) Подставить query params.
3) Отправить запрос.
4) Проверить статус и структуру списка платежей.

Expected Result:
- 200.
- Возвращается список платежей (может быть пустым), структура по контракту.

Postconditions: —
Attachments:
- Postman: `api/v1/subscriptions/my-payments/...`

---

## TC-SUB-003
Title: Subscription info (authorized) — текущее состояние подписки  
US: US-SUB-3  
Module: Subscriptions  
Type: Functional / Contract  
Priority: P2  
Severity: Minor  
Endpoint: GET `/api/v1/subscriptions/info`

Preconditions:
- Есть `{{bearerToken}}`.

Test Data: —
Steps:
1) Открыть запрос **Subscription info**.
2) Отправить запрос.
3) Проверить статус и поля статуса подписки (активна/не активна/план/даты — по контракту).

Expected Result:
- 200.
- Поля статуса подписки соответствуют контракту.

Postconditions: —

Attachments:
- Postman: `api/v1/subscriptions/info/...`

---

## TC-SUB-004
Title: Disable auto renewal (authorized)  
US: US-SUB-4  
Module: Subscriptions  
Type: Functional  
Priority: P2  
Severity: Minor  
Endpoint: POST `/api/v1/subscriptions/disable-auto-renewal`

Preconditions:
- Есть `{{bearerToken}}`.
- Подписка активна и авто-продление включено.

Test Data: —
Steps:
1) Открыть запрос **Disable auto renewal**.
2) Отправить запрос.
3) Проверить статус.
4) Выполнить TC-SUB-003 и проверить, что autoRenewal выключен (по контракту).

Expected Result:
- 200.
- Автопродление выключено (подтверждается `info`).

Postconditions: —

Attachments:
- Postman: `api/v1/subscriptions/disable-auto-renewal/...`

---

## TC-SUB-005
Title: Enable auto renewal (authorized)  
US: US-SUB-5  
Module: Subscriptions  
Type: Functional  
Priority: P2  
Severity: Minor  
Endpoint: POST `/api/v1/subscriptions/enable-auto-renewal`

Preconditions:
- Есть `{{bearerToken}}`.
- Подписка активна (если требуется).

Test Data: —
Steps:
1) Открыть запрос **Enable auto renewal**.
2) Отправить запрос.
3) Проверить статус.
4) Выполнить TC-SUB-003 и проверить флаг авто-продления.

Expected Result:
- 200.
- Автопродление включено (по контракту).

Postconditions: —

Attachments:
- Postman: `api/v1/subscriptions/enable-auto-renewal/...`

---

## TC-SUB-006
Title: Cancel current user subscription (testing)  
US: US-SUB-6  
Module: Subscriptions  
Type: Functional / Test-only  
Priority: P3  
Severity: Minor  
Endpoint: DELETE `/api/v1/subscriptions/testing/cancel-subscription`

Preconditions:
- Есть `{{bearerToken}}`.
- Эндпоинт тестовый и может отсутствовать на prod.

Test Data: —
Steps:
1) Открыть запрос **Cancel current user subscription**.
2) Отправить запрос.
3) Проверить статус.
4) Выполнить TC-SUB-003 и проверить, что подписка отменена/не активна (по контракту).

Expected Result:
- 200.
- Подписка отменена (по контракту).

Postconditions: —

Attachments:
- Postman: `api/v1/subscriptions/testing/cancel-subscription/...`

---

## TC-SUB-007
Title: Get payments (testing) — история платежей (test endpoint)  
US: US-SUB-7  
Module: Subscriptions  
Type: Functional / Test-only  
Priority: P3  
Severity: Trivial  
Endpoint: GET `/api/v1/subscriptions/testing/my-payments?...`

Preconditions:
- Есть `{{bearerToken}}`.

Test Data:
- pageSize: `10`
- pageNumber: `1`

Steps:
1) Открыть запрос **Get payments** (testing).
2) Подставить query params.
3) Отправить запрос.
4) Проверить статус и структуру.

Expected Result:
- 200.
- Предсказуемая структура списка платежей (по контракту).

Postconditions: —

Attachments:
- Postman: `api/v1/subscriptions/testing/my-payments/...`
