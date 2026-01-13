# COUNTRY CATALOG — API Test Cases

Версия: 1.0  
Окружение: Postman/Newman  
Модуль: Country Catalog

---

## TC-LOC-001
Title: Get list of countries (public)  
US: US-LOC-1  
Module: Country Catalog  
Type: Functional / Contract  
Priority: P3  
Severity: Trivial  
Endpoint: GET `/api/v1/country-catalog/country`

Preconditions: —
Test Data: —

Steps:
1) Открыть запрос **Get list of countries**.
2) Отправить запрос.
3) Проверить статус и структуру списка стран.

Expected Result:
- 200/2xx.
- Возвращается список стран, структура по контракту.

Postconditions:
- Сохранить `countryId` для TC-LOC-002.

Attachments:
- Postman: `api/v1/country-catalog/country/...`

---

## TC-LOC-002
Title: Get list of cities by countryId (public)  
US: US-LOC-2  
Module: Country Catalog  
Type: Functional / Contract  
Priority: P3  
Severity: Trivial  
Endpoint: GET `/api/v1/country-catalog/{countryId}/city`

Preconditions:
- Выполнен TC-LOC-001 и есть `countryId`.

Test Data:
- countryId: `<country_id>`

Steps:
1) Открыть запрос **Get list of cities**.
2) Подставить `countryId`.
3) Отправить запрос.
4) Проверить структуру списка городов.

Expected Result:
- 200/2xx (или 404 для несуществующего countryId — по контракту).
- Возвращается список городов, структура соответствует контракту.

Postconditions: —

Attachments:
- Postman: `api/v1/country-catalog/{countryId}/city/...`
