# NOTIFICATIONS — API Test Cases

Версия: 1.0  
Окружение: Postman/Newman + WS client (для части сценариев)  
Модуль: Notifications

---

## TC-NOTIF-001
Title: Get notifications (authorized) — уведомления за период  
US: US-NOTIF-1  
Module: Notifications  
Type: Functional / Contract  
Priority: P2  
Severity: Major  
Endpoint: GET `/api/v1/notifications`

Preconditions:
- Есть `{{bearerToken}}`.

Test Data: —
Steps:
1) Открыть запрос **Get notifications of the current user (for 1 month)**.
2) Отправить запрос с Bearer token.
3) Проверить status code и структуру массива (может быть пустым).

Expected Result:
- 200/2xx.
- Ответ содержит список уведомлений (или пустой список), структура элементов соответствует контракту.
- Нет 500.

Postconditions:
- Сохранить `notificationId` одного уведомления для TC-NOTIF-002 (если есть).

Attachments:
- Postman: `api/v1/notifications/Get notifications...`

---

## TC-NOTIF-002
Title: Read notification by id (positive) — пометить как прочитанное  
US: US-NOTIF-2  
Module: Notifications  
Type: Functional  
Priority: P2  
Severity: Minor  
Endpoint: PUT `/api/v1/notifications/read/{notificationId}`

Preconditions:
- Выполнен TC-NOTIF-001 и получен `notificationId`.

Test Data:
- notificationId: `<id_from_TC-NOTIF-001>`

Steps:
1) Открыть запрос **Read notification by id for current user**.
2) Подставить `notificationId`.
3) Отправить запрос.
4) Повторить TC-NOTIF-001 и проверить, что у уведомления изменился признак `read`/`isRead` (по контракту), уведомление пропало из непрочитанных.

Expected Result:
- 200/2xx.
- Состояние уведомления меняется согласно контракту.

Postconditions: —

Attachments:
- Postman: `api/v1/notifications/read/{notificationId}/...`

---

## TC-NOTIF-003
Title: Testing send new notification into websocket (smoke)  
US: US-NOTIF-3  
Module: Notifications  
Type: Integration-sanity  
Priority: P3  
Severity: Minor  
Endpoint: GET `/api/v1/notifications/testing`

Preconditions:
- (Опционально) есть активное WS-подключение (TC-WSS-001).

Test Data: —
Steps:
1) Открыть запрос **Testing send new notification into websocket connection**.
2) Отправить запрос.
3) Зафиксировать статус.
4) Если WS подключен — проверить приход события/сообщения в сокет.

Expected Result:
- 200/2xx или предсказуемый ответ тестового эндпоинта.
- При активном WS уведомление доставляется в канал.

Postconditions: —

Attachments:
- Postman: `api/v1/notifications/testing/...`
