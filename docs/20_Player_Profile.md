# 20. Player Profile

## Purpose

Player Profile — центральный агрегат цифровой футбольной личности пользователя.

В отличие от Match, который существует ограниченное время, Player Profile существует на протяжении всей футбольной карьеры пользователя и является накопителем объективной игровой истории.

Каждый завершённый матч обновляет Player Profile.

Player Profile является единственным источником информации для построения цифровой репутации игрока.

---

# Responsibilities

Player Profile отвечает за:

- общий рейтинг игрока;
- рейтинги по игровым позициям;
- статистику матчей;
- достижения;
- историю эволюции;
- предпочтительные позиции;
- игровые специализации;
- формирование Snapshot для новых матчей.

Player Profile не отвечает за проведение матчей.

Матч лишь генерирует события, которые изменяют Player Profile.

---

# Lifecycle

```
User Registration

↓

Player Profile Created

↓

Match Completed

↓

Evolution Pipeline

↓

Player Profile Updated

↓

Next Match

↓

...
```

Player Profile существует постоянно.

Он никогда не пересоздаётся между матчами.

---

# Player Evolution Pipeline

После каждого завершённого матча запускается Player Evolution Pipeline.

```
Match

↓

Protocol

↓

Performance Analysis

↓

Rating Calculation

↓

Statistics Update

↓

Achievements Update

↓

Player Profile Updated
```

Каждый этап является самостоятельным Engine.

---

# Snapshot Principle

Перед запуском любого Evolution Engine создаётся Snapshot текущего состояния Player Profile.

Все вычисления производятся исключительно на основе Snapshot.

После завершения вычислений создаётся новая версия Player Profile.

История изменений не изменяется.

Данный принцип описан в ADR-006.

---

# Core Components

Player Profile включает:

- Overall Rating
- Position Ratings
- Preferred Positions
- Match Statistics
- Achievements
- Reputation
- Match History Summary

---

# Overall Rating

Overall Rating отражает общий уровень игрока.

Используется:

- при поиске матчей;
- при формировании команд;
- в рейтингах;
- в статистике.

Overall Rating не редактируется вручную.

Изменяется исключительно Evolution Engine.

---

# Position Ratings

Для каждой игровой позиции хранится собственный рейтинг.

Например:

- Goalkeeper
- Defender
- Midfielder
- Forward

Position Rating изменяется независимо.

---

# Preferred Positions

Player Profile хранит упорядоченный список предпочтительных позиций.

Например:

1. Forward
2. Midfielder
3. Defender
4. Goalkeeper

Этот список используется Formation Engine.

В будущем может автоматически корректироваться на основе истории матчей.

---

# Statistics

Player Profile содержит накопительную статистику.

Например:

- Matches Played
- Wins
- Draws
- Losses
- Goals
- Assists
- Saves
- Clean Sheets
- Yellow Cards
- Red Cards
- MVP Awards

Список статистических показателей расширяем.

---

# Achievements

Player Profile хранит все полученные достижения.

Каждое достижение имеет:

- тип;
- дату получения;
- источник;
- условия получения.

Achievements никогда не удаляются.

---

# Reputation

Reputation отражает доверие системы к игроку.

На Reputation могут влиять:

- подтверждённые участия;
- отмены регистраций;
- неявки;
- дисциплинарные нарушения;
- оценки судей;
- оценки организаторов.

Reputation рассчитывается автоматически.

---

# Formation Candidate

Player Profile не участвует напрямую в Formation Engine.

Перед началом формирования команд создаётся FormationCandidate.

```
Player Profile

↓

FormationCandidateBuilder

↓

FormationCandidate

↓

Formation Engine
```

FormationCandidate является Snapshot Player Profile на момент начала матча.

---

# Business Rules

# BR-PLAYER-001

Player Profile создаётся автоматически при назначении пользователю роли Player.

Каждый пользователь может иметь не более одного Player Profile.

Player Profile существует независимо от других ролей пользователя и сопровождает его на протяжении всей футбольной карьеры.

---

# BR-PLAYER-002

Каждый завершённый матч запускает Player Evolution Pipeline.

---

# BR-PLAYER-003

Player Profile изменяется только через Evolution Engine.

---

# BR-PLAYER-004

История матчей неизменяема.

---

# BR-PLAYER-005

Все изменения Player Profile вычисляются автоматически.

Ручное изменение запрещено.

---

# BR-PLAYER-006

Formation Engine никогда не работает непосредственно с Player Profile.

Используется только FormationCandidate.

---

# BR-PLAYER-007

Перед каждым Evolution Engine создаётся Snapshot текущего состояния Player Profile.

---

# BR-PLAYER-008

Существуют разные этапы жизни Игрока (U12, U16, Adult, Veterans)

Переход из Adult в Veterans является необратимым. Это исключает манипуляции рейтингами.

---

# Long-term Vision

Player Profile является основой цифровой футбольной репутации пользователя.

В долгосрочной перспективе именно Player Profile становится главным носителем ценности платформы.

Матчи являются событиями, изменяющими Player Profile.

Таким образом:

User → Player Profile → Match History → Reputation → Evolution

является основной моделью цифровой футбольной экосистемы Footforall.
