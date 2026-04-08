# C4 и архитектура безопасности

## 1. Context

Система взаимодействует с:
- пациентами
- сотрудниками
- лабораторией
- 1С

---

## 2. Container

Основные компоненты:
- API Gateway
- IAM (OIDC)
- CRM
- Medical Service
- Billing
- Notification

---

## 3. Security Architecture

### Уровни:

1. Access
    - WAF
    - API Gateway

2. Identity
    - OIDC
    - MFA

3. Policy
    - RBAC / ABAC

4. Data
    - encryption
    - tagging

5. Monitoring
    - SIEM
    - audit

---

## 4. Принципы

- Zero Trust
- Least Privilege
- Privacy by Design

---

## 5. Вывод

Архитектура обеспечивает:
- защиту данных
- контроль доступа
- масштабируемость