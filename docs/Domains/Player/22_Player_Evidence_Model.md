# 22. Player Evidence Model

## Purpose

Player Evidence Model определяет единый формат объективных игровых доказательств (Evidence), получаемых после завершения каждого матча.

Evidence является единственным входом для всех Evolution Engine.

Ни один Engine не анализирует Match или Protocol напрямую.

Все Engine работают исключительно с Evidence.

---

# Motivation

После завершения матча система уже знает множество фактов.

Например:

- игрок вышел на поле;
- сыграл 90 минут;
- забил гол;
- получил жёлтую карточку;
- признан MVP;
- команда победила.

Однако сами по себе события матча ещё не изменяют Player Profile.

Сначала события преобразуются в объективные доказательства (Evidence).

Только после этого запускаются Evolution Engine.

---

# Evolution Pipeline

```
Match

↓

Protocol

↓

Evidence Builder

↓

Evidence Package

↓

Evolution Engines

↓

Player Profile
```

---

# Source of Truth

Источником всех Evidence являются исключительно:

- завершённый Match;
- утверждённый Match Protocol.

Evidence никогда не создаются вручную.

---

# Evidence Principles

Evidence являются:

- объективными;
- неизменяемыми;
- воспроизводимыми;
- независимыми от алгоритмов расчёта рейтингов.

Любой Engine может быть полностью переписан без изменения уже существующих Evidence.

---

# Evidence Package

Для каждого участника матча формируется отдельный Evidence Package.

Например:

```
Player Evidence Package

Player Id

Match Id

Protocol Version

Match Trust Score

Evidence[]
```

---

# Evidence

Каждый Evidence описывает один подтверждённый факт.

Например:

```
Goal Scored

Minute 56

Trust 0.97
```

или

```
Played Minutes

90

Trust 1.00
```

или

```
Yellow Card

Minute 81

Trust 0.99
```

---

# Evidence Categories

Evidence делятся на несколько групп.

## Participation

Факты участия игрока.

Например:

- Match Played
- Minutes Played
- Started Match
- Substitute
- Finished Match

---

## Performance

Игровые действия.

Например:

- Goal
- Assist
- Save
- Clean Sheet
- Own Goal

---

## Discipline

Например:

- Yellow Card
- Red Card
- Suspension

---

## Team Result

Например:

- Win
- Draw
- Loss
- Tournament Winner

---

## Awards

Например:

- MVP
- Best Goal
- Fair Play

---

## Reputation

Например:

- Confirmed Participation
- No Show
- Cancelled Registration
- Fair Behaviour

---

# Match Trust

Каждый Evidence сопровождается Match Trust Score.

Таким образом одинаковое игровое действие может иметь различный вес в зависимости от условий проведения матча.

Данный механизм описан в ADR-008.

---

# Engine Independence

Каждый Evolution Engine самостоятельно принимает решение, каким образом использовать полученные Evidence.

Например:

Rating Engine анализирует:

- Goals
- Assists
- Minutes
- Saves

Reputation Engine анализирует:

- Confirmed Participation
- No Show
- Fair Play
- Discipline

Achievement Engine анализирует:

- Hat-trick
- Clean Sheet
- Tournament Winner

Statistics Engine анализирует:

- все накопительные показатели.

Ни один Engine не зависит от реализации другого Engine.

---

# Immutability

После создания Evidence не изменяются.

Если протокол был исправлен, создаётся новый набор Evidence.

Предыдущая версия сохраняется в истории.

---

# Snapshot Principle

Перед запуском любого Evolution Engine создаётся Snapshot Player Profile.

Engine получает:

- Snapshot Player Profile;
- Evidence Package.

На выходе формируется новая версия Player Profile.

---

# Long-term Vision

Evidence являются фундаментальным слоем цифровой футбольной экосистемы Footforall.

Все алгоритмы платформы работают не с матчами напрямую, а с объективными доказательствами, извлечёнными из утверждённых протоколов.

Это позволяет:

- изменять алгоритмы рейтингов без потери истории;
- пересчитывать Player Profile при изменении моделей оценки;
- обеспечивать полную воспроизводимость результатов;
- использовать единый источник данных для всех Evolution Engine.
