# Целевая архитектура потоков данных (TO-BE)

## 1. Принципы

- централизованное хранение
- API-first
- security by design
- минимизация данных

---

## 2. Потоки

### Запись пациента
Пациент → Portal → API Gateway → CRM

---

### Приём врача
Doctor → Medical Service → DB → Audit

---

### Лаборатория
Service → API Gateway → Lab API

---

### Оплата
Portal → Payment Gateway → Billing

---

## 3. Контроль

- OAuth2 / OIDC
- RBAC / ABAC
- encryption
- audit

---

## 4. Вывод

TO-BE архитектура:
- безопасна
- масштабируема
- управляемая