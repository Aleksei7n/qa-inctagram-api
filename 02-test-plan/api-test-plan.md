# API Test Plan — Inctagram/Somegram (v1)

## 1) Objective
Проверить корректность работы ключевых API-сценариев: Auth, Users/Profile, Posts, Social (follow), Public endpoints, Subscriptions, Country catalog + базовые проверки контрактов/ошибок.

## 2) Scope

### In scope
- Auth: registration, confirmation, login, refresh-token, logout, password recovery, new password, GitHub callback
- Devices: list + terminate
- Users: profile CRUD, avatar, change email + confirmation, change password, followers/following, follow/unfollow
- Posts: CRUD, like/unlike, list by user
- Public-users / Public-posts
- Notifications
- Subscriptions
- Country catalog
- WSS: smoke connectivity (без глубокой функциональности)

### Out of scope
- Нагрузочное тестирование, аудит кода
- Проверка реальных платёжных провайдеров
- Мобильные пуши/доставка уведомлений (только API “get list”)

## 3) Test types
- Functional (positive/negative)
- Contract checks (минимальная схема/обязательные поля)
- Auth & session management (token lifecycle)
- Basic security sanity (401/403, input validation)
- Smoke / Regression (чек-листы)

## 4) Test approach
- Основной инструмент: Postman (коллекция + окружение)
- Переменные окружения: `baseUrl`, `bearerToken`, тестовые userId/postId и т.п.
- Newman: CLI прогон коллекции + отчёт
- Для нестабильных данных: предусмотреть “setup” шаги (создать тестового пользователя/пост)

## 5) Environment
- BASE_URL
- Auth: Bearer token в `{{bearerToken}}`
- Данные: тестовый пользователь

## 6) Entry / Exit criteria
### Entry
- Стенд доступен
- Есть тестовый пользователь или возможность регистрации
- Подготовлено окружение Postman (baseUrl)

### Exit
- Выполнен smoke
- Пройден основной набор тест-кейсов
- Зафиксированы дефекты
- Сформирован Test Summary Report

## 7) Risks / assumptions
- Регистрация/почта/подтверждение могут зависеть от внешних сервисов (нестабильно на стенде)
- Токены могут быть короткоживущими → flaky для refresh/logout кейсов
- Данные в БД могут “шуметь” (посты/юзеры уже существуют)

## 8) Deliverables
- Requirements/Inventory
- Test Plan
- Checklists (smoke/regression/security sanity)
- Test Cases
- Bug reports (template + examples)
- Test Summary Report
- Traceability Matrix
- Postman/Newman artifacts
