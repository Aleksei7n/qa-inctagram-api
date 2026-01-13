# DEVICES — API Test Cases

Версия: 1.0  
Окружение: Postman/Newman  
Модуль: Devices/Sessions

---

## TC-DEV-001
Title: Get all active sessions (authorized)  
US: US-DEV-1  
Module: Devices  
Type: Functional / Security-sanity  
Priority: P2  
Severity: Major  
Endpoint: GET `/api/v1/devices`

Preconditions:
- Выполнен TC-AUTH-009 (есть `{{bearerToken}}`).

Test Data: —
Steps:
1) Открыть запрос **Get all active sessions of the current user**.
2) Отправить запрос с Bearer token.
3) Проверить status code и тело ответа.

Expected Result:
- 200/2xx.
- Возвращается список активных сессий/устройств.
- Каждый элемент содержит идентификатор сессии/устройства (по контракту).

Postconditions:
- Сохранить один `deviceId` для TC-DEV-002.

Attachments:
- Postman: `api/v1/devices/Get all active sessions...`

---

## TC-DEV-002
Title: Terminate session by deviceId (positive)  
US: US-DEV-2  
Module: Devices  
Type: Functional  
Priority: P2  
Severity: Major  
Endpoint: DELETE `/api/v1/devices/terminate/{deviceId}`

Preconditions:
- Выполнен TC-DEV-001, есть валидный `deviceId`.

Test Data:
- deviceId: `<deviceId_from_TC-DEV-001>`

Steps:
1) Открыть запрос **End session by ID**.
2) Подставить `deviceId` в path param.
3) Отправить запрос.
4) Повторить TC-DEV-001 и проверить, что завершённая сессия исчезла.

Expected Result:
- 204/2xx по контракту.
- Завершённая сессия больше не возвращается в списке.

Postconditions: —

Attachments:
- Postman: `api/v1/devices/terminate/{deviceId}/End session by ID`

---

## TC-DEV-003
Title: Terminate all sessions except current (positive)  
US: US-DEV-3  
Module: Devices  
Type: Functional  
Priority: P2  
Severity: Major  
Endpoint: DELETE `/api/v1/devices/terminate`

Preconditions:
- Есть минимум 1 дополнительная сессия (если возможно создать).

Test Data: —
Steps:
1) Открыть запрос **End all sessions except the current one**.
2) Отправить запрос с Bearer token.
3) Выполнить TC-DEV-001 и проверить, что осталась только текущая сессия.

Expected Result:
- 204/2xx.
- Дополнительные сессии завершены (по контракту).

Postconditions: —

Attachments:
- Postman: `api/v1/devices/terminate/End all sessions...`
