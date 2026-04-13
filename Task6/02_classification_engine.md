# Движок классификации данных

## 1. Контейнеры решения (C2)

Предлагаемый C2-уровень движка включает следующие контейнеры:

### 1. Source Connectors
Принимают batch-данные из:
- CRM / Patient Service;
- Booking Service;
- Medical Record Service;
- Billing Service;
- Notification Service;
- Lab Integration;
- 1С и файловых выгрузок.

### 2. Landing / Raw Intake
Временная зона приема сырых данных.
Используется только для краткоживущего хранения входящих наборов до момента классификации.

### 3. Schema Registry & Profiler
Отвечает за:
- обнаружение структуры;
- сравнение версий схем;
- выявление новых полей;
- профильный анализ содержимого.

### 4. Classification Engine
Сердце решения. Выполняет:
- rule-based классификацию;
- dictionary / regex matching;
- semantic detection;
- ML-assisted classification;
- confidence scoring;
- field-level tagging.

### 5. Privacy Policy Engine
На основе класса данных принимает решение:
- можно ли загружать набор дальше;
- какие transformation steps обязательны;
- в какой слой хранилища допустима загрузка;
- требуется ли ручная валидация при низкой уверенности.

### 6. Privacy Transformation Service
Применяет:
- masking;
- tokenization;
- pseudonymization;
- anonymization;
- suppression / dropping of fields.

### 7. Metadata, Catalog & Lineage Store
Хранит:
- реестр наборов данных;
- версии схем;
- data owners;
- tags;
- confidence score;
- lineage;
- policy decisions.

### 8. Curated Analytical Storage
Состоит из нескольких privacy-aware зон:
- Restricted Raw Zone
- Trusted Curated Zone
- Pseudonymized Analytics Zone
- Anonymized BI/ML Zone

### 9. Access Layer
Предоставляет controlled access для:
- BI;
- ML;
- AI / LLM;
- аналитиков;
- дата-сайентистов.

---

## 2. Как работает классификация

## 2.1 Источники правил
Классификация строится на сочетании нескольких механизмов:

### A. Structural heuristics
Примеры:
- столбцы с именами `phone`, `email`, `birth_date`, `passport`, `diagnosis`, `lab_result`, `policy_number`;
- типы полей;
- отношения между полями.

### B. Pattern matching
- email regex;
- phone patterns;
- passport / SNILS / policy number patterns;
- медицинские коды и словари;
- признаки адресов и документов.

### C. Semantic dictionaries
- словари медицинских терминов;
- словари финансовых атрибутов;
- словари персональных идентификаторов;
- каталоги справочников и служебных данных.

### D. ML/AI-assisted classification
Используется как дополнительный механизм, если:
- пришел новый нестандартный набор полей;
- названия колонок неинформативны;
- есть текстовые документы, полу-структурированные payload или свободный текст.

### E. Manual review queue
При низкой уверенности классификации набор отправляется на review владельцу данных / data steward / security analyst.

---

## 3. Выход классификатора

Для каждого набора данных и для каждого поля движок формирует:

- `data_domain` — домен данных (patient, medical, billing, operations, reference);
- `confidentiality_class` — PUBLIC / INTERNAL / PII / MEDICAL / FINANCE;
- `sensitivity_level` — LOW / MEDIUM / HIGH / CRITICAL;
- `required_controls` — masking / tokenization / anonymization / encrypt_only / drop;
- `allowed_zones` — какие слои storage разрешены;
- `confidence_score` — уверенность классификации;
- `schema_version` — версия схемы;
- `policy_decision` — allow / transform / quarantine / manual review.

---

## 4. Реакция на schema drift

Так как входные структуры подвержены изменениям, движок должен поддерживать режим schema evolution:

1. Новое поле обнаружено автоматически.
2. Поле профилируется на sample-данных.
3. Классификатор определяет его класс и confidence score.
4. Если уверенность высокая — применяется автоматическое решение.
5. Если уверенность низкая — загрузка набора в продуктивные аналитические зоны блокируется,
   а набор помещается в quarantine / review.

Это позволяет не ломать весь pipeline при изменении схемы, но и не допускать бесконтрольную загрузку новых полей.

