# ADR-001

## Title

Domain First Development

---

## Status

Accepted

---

## Context

Проект Footforall разрабатывается по принципам Domain Driven Design.

Главной целью является построение корректной модели предметной области до реализации инфраструктуры.

---

## Decision

Разработка каждого нового домена выполняется в следующем порядке:

1. Business Rules;
2. Domain Model;
3. State Machine;
4. Validation;
5. Application Services;
6. Tests;
7. Infrastructure;
8. REST API.

---

## Consequences

Преимущества:

- бизнес-логика независима от технологий;
- высокая тестируемость;
- простая замена инфраструктуры;
- стабильная архитектура.

Недостатки:

- более длительное проектирование;
- инфраструктура появляется позже.
