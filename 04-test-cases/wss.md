# WSS / WebSocket Notifications — Test Cases

Версия: 1.0  
Окружение: Postman WS client / любой WS клиент  
Модуль: WSS Notifications

---

## TC-WSS-001
Title: Connection and listen notifications — успешное подключение и получение событий  
US: US-WSS-1  
Module: WSS  
Type: Integration-sanity  
Priority: P2  
Severity: Major  
Endpoint: WSS `wss://somegram.online/notification`

Preconditions:
- WS endpoint доступен из сети.
- Еесть токен/куки/параметры авторизации, необходимые для подключения.

Test Data:
- auth: `Bearer {{bearerToken}}`

Steps:
1) Открыть WS запрос **Connection and listen notifications...**
2) Подключиться к `wss://somegram.online/notification`.
3) Убедиться, что handshake прошёл успешно (connected).
4) Выполнить TC-NOTIF-003 (send testing notification) или любое действие, генерирующее уведомление.
5) Проверить, что в WS приходит сообщение/ивент о новом уведомлении.
6) Зафиксировать пример payload.

Expected Result:
- WS соединение устанавливается.
- При генерации уведомления клиент получает сообщение в WS.
- Нет бесконечных реконнектов/ошибок без причины.

Postconditions:
- Разорвать соединение.

Attachments:
- Postman WS: `wss://somegram.online/notification` (message log)
- (optional) `assets/wss-logs/TC-WSS-001.txt`

---

## TC-WSS-002
Title: Exceptions (negative) — предсказуемая обработка ошибок WS  
US: US-WSS-2  
Module: WSS  
Type: Negative / Stability-sanity  
Priority: P3  
Severity: Minor  
Endpoint: `wss://somegram.online/notification/error` / “Exceptions”

Preconditions:
- Доступ к тестовому endpoint/сценарию ошибок.

Test Data: —
Steps:
1) Открыть запрос **Exceptions**.
2) Выполнить действие, которое должно вызвать ошибку/исключение.
3) Зафиксировать статус/ответ (или событие в WS).
4) Проверить, что ошибка оформлена корректно (код/сообщение), без падения сервиса.

Expected Result:
- Ошибка возвращается предсказуемо (по контракту).
- Нет 500/нечитаемых stack traces наружу.

Postconditions: —

Attachments:
- Postman: `wss ... /error (Exceptions)` (лог/ответ)
