# Traceability Matrix — Inctagram/Somegram API (v1)

Версия: 1.1  
Источник: `08-postman/collection.json` + Swagger  
Цель: связать высокоуровневые требования/фичи с тестовым покрытием (TC) и критичными дефектами (BUG).

---

## Legend
- **US-*** — Requirement / User Story (уровень фичи/модуля)
- **TC-*** — Test Case ID
- **BUG-*** — Defect ID (если найдено)

---

| Requirement (US) | Coverage (Test Cases) | Related Defects |
|---|---|---|
| US-AUTH-01 Registration | TC-AUTH-001..TC-AUTH-002 | BUG-002 |
| US-AUTH-02 Registration confirmation | TC-AUTH-003..TC-AUTH-004 | — |
| US-AUTH-03 Login | TC-AUTH-005..TC-AUTH-006 | — |
| US-AUTH-04 Refresh token | TC-AUTH-007..TC-AUTH-008 | — |
| US-AUTH-05 Logout | TC-AUTH-009..TC-AUTH-010 | BUG-001 |
| US-AUTH-06 Recovery / New password | TC-AUTH-011..TC-AUTH-013 | — |
| US-DEV-01 Devices sessions | TC-DEV-001..TC-DEV-005 | — |
| US-NOTIF-01 Notifications | TC-NOTIF-001..TC-NOTIF-002 | — |
| US-USER-01 Profile management | TC-USER-001..TC-USER-011 | — |
| US-SOC-01 Followers / Following | TC-USER-012..TC-USER-019, TC-FOLL-001..TC-FOLL-003 | — |
| US-POST-01 Posts (CRUD/likes) | TC-POST-001..TC-POST-010 | BUG-003 |
| US-PUBLIC-01 Public access (users/posts) | TC-PUBU-001..TC-PUBU-003, TC-PUBP-001..TC-PUBP-005 | — |
| US-LOC-01 Country catalog | TC-LOC-001..TC-LOC-003 | — |
| US-SUB-01 Subscriptions | TC-SUB-001..TC-SUB-004 | — |
| US-WSS-01 WSS smoke | TC-WSS-001 | — |
