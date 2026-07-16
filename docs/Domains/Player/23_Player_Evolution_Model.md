# 23. Player Evolution Model

## Purpose

Player Evolution Model определяет единый механизм изменения Player Profile.

Все изменения цифровой личности игрока выполняются исключительно через Evolution Engine.

Никакие другие компоненты системы не имеют права изменять Player Profile напрямую.

---

# Principle

Player Profile является неизменяемым источником текущего состояния.

Каждое изменение создаёт новую версию состояния.

Предыдущие версии сохраняются в истории.

---

# Evolution Flow

```
Player Profile Snapshot

↓

Evidence Package

↓

Evolution Engine

↓

Evolution Result

↓

New Player Profile

↓

Evolution History
```

Все Engine используют одинаковый жизненный цикл.

---

# Snapshot

Перед запуском любого Engine создаётся Snapshot текущего Player Profile.

Snapshot гарантирует, что все вычисления производятся относительно одного и того же состояния.

Во время работы Engine Snapshot не изменяется.

---

# Evidence

Единственным входом Evolution Engine является Evidence Package.

Engine никогда не работает напрямую с Match или Protocol.

---

# Evolution Result

Engine не изменяет Player Profile.

Engine возвращает Evolution Result.

Например:

```
Overall Rating

84 → 85

Reason

Goal
```

или

```
Reputation

92 → 91

Reason

No Show
```

Evolution Result содержит только изменения.

---

# Applying Changes

После завершения всех Engine система объединяет полученные Evolution Result.

Только после этого создаётся новая версия Player Profile.

---

# Atomicity

Обновление Player Profile является атомарной операцией.

Либо применяются результаты всех Engine.

Либо не применяется ни один.

---

# Evolution History

После успешного обновления создаётся запись Evolution History.

Каждая запись содержит:

- Snapshot Version
- Engine
- Applied Changes
- Timestamp

История никогда не изменяется.

---

# Engine Independence

Каждый Engine отвечает только за собственную область.

Например:

Rating Engine

↓

Rating

Reputation Engine

↓

Reputation

Achievement Engine

↓

Achievements

Statistics Engine

↓

Statistics

Engine не взаимодействуют друг с другом напрямую.

---

# Conflict Resolution

Если несколько Engine изменяют разные части Player Profile, изменения объединяются автоматически.

Если несколько Engine пытаются изменить один и тот же показатель, применяется заранее определённая стратегия разрешения конфликтов.

Такие ситуации должны быть исключением.

---

# Idempotency

Повторный запуск Evolution Engine с одинаковыми Snapshot и Evidence должен приводить к одинаковому результату.

Это обеспечивает возможность полного пересчёта Player Profile.

---

# Rebuild

Player Profile может быть полностью восстановлен.

```
Player Created

↓

Evolution History Replay

↓

Current Player Profile
```

Таким образом Player Profile является воспроизводимым состоянием.

---

# Long-term Vision

Все изменения цифровой личности игрока представляют собой последовательность независимых Evolution.

Player Profile является текущим состоянием этой последовательности.

История Evolution является единственным источником восстановления состояния при изменении алгоритмов платформы.
