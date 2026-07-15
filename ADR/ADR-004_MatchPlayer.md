# ADR-004

## Title

Temporary MatchPlayer Model

---

## Status

Accepted

---

## Context

Алгоритм формирования команд использует сущность MatchPlayer.

MatchPlayer содержит:

- userId;
- position;
- rating;
- teamId.

После реализации Player Performance Profile рейтинг игрока будет рассчитываться отдельно для каждой игровой позиции.

---

## Decision

До завершения MVP оставить MatchPlayer входной моделью алгоритма формирования команд.

После реализации Player Performance Profile заменить MatchPlayer на FormationCandidate.

FormationCandidate будет автоматически строиться на основании:

- MatchRegistration;
- рейтингов по игровым позициям;
- приоритетов игровых позиций пользователя.

---

## Consequences

Преимущества:

- MVP остаётся простым;
- алгоритм формирования команд не усложняется.

Недостатки:

- потребуется рефакторинг TeamFormationService.
