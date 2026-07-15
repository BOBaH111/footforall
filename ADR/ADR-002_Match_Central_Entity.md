# ADR-002

## Title

Match as Central Transaction Entity

---

## Status

Accepted

---

## Context

Платформа объединяет игроков, судей и владельцев площадок.

Необходимо определить центральную сущность Transaction Domain.

---

## Decision

Центральной сущностью Transaction Domain является Match.

Все остальные сущности описывают жизненный цикл конкретного матча.

К ним относятся:

- Registration;
- Team Formation;
- Team Slot Assignment;
- Match Events;
- Match Protocol.

---

## Consequences

Матч становится единственным источником данных для последующей аналитики.

Все изменения цифрового состояния участников происходят только после завершения матча.
