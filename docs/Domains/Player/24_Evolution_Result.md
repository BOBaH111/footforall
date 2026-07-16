# 24. Evolution Result

## Purpose

Evolution Result является единым форматом результата работы любого Evolution Engine.

Все Engine платформы возвращают исключительно Evolution Result.

Таким образом Player Evolution Pipeline не зависит от реализации конкретных Engine.

---

# Principle

Engine никогда не изменяет Player Profile напрямую.

Engine анализирует Snapshot Player Profile и Evidence Package.

Результатом работы Engine является только описание необходимых изменений.

---

# Evolution Flow

```
Snapshot

+

Evidence Package

↓

Evolution Engine

↓

Evolution Result
```

---

# Evolution Result

Evolution Result описывает изменение одного или нескольких компонентов Player Profile.

Например:

```
Overall Rating

84

↓

85
```

или

```
Goals

24

↓

25
```

или

```
Reputation

91

↓

90
```

---

# Evolution Change

Каждый Evolution Result состоит из набора Evolution Change.

Каждый Evolution Change содержит:

- Target
- Previous Value
- New Value
- Delta
- Reason
- Source
- Trust Score

---

# Target

Определяет изменяемый компонент.

Например:

- Overall Rating
- Position Rating
- Reputation
- Statistics
- Achievement

---

# Previous Value

Значение до запуска Evolution Engine.

---

# New Value

Новое рассчитанное значение.

---

# Delta

Изменение относительно предыдущего значения.

Например:

+2

-1

+15

---

# Reason

Причина изменения.

Например:

Goal

Assist

Hat-trick

No Show

Season Completed

Rating Decay

---

# Source

Источник изменения.

Например:

Match Evolution Engine

Time Evolution Engine

Achievement Engine

Reputation Engine

---

# Trust Score

Каждый Evolution Change содержит Match Trust Score либо другой Trust Score, использованный при расчёте.

Это обеспечивает полную воспроизводимость вычислений.

---

# Multiple Changes

Один Engine может вернуть несколько Evolution Change.

Например:

```
Goal

↓

Overall Rating +1

↓

Forward Rating +2

↓

Goals +1

↓

Minutes +90
```

---

# Applying Results

Player Evolution Pipeline объединяет Evolution Result всех Engine.

После успешной агрегации создаётся новая версия Player Profile.

---

# Engine Independence

Pipeline ничего не знает о конкретных Engine.

Для Pipeline существует только один контракт.

```
Snapshot

↓

Engine

↓

Evolution Result
```

Таким образом любой новый Engine автоматически становится частью Evolution Pipeline.

---

# Immutability

Evolution Result никогда не изменяется после создания.

После применения сохраняется в Evolution History.

---

# Replay

Любая версия Player Profile может быть восстановлена путём последовательного применения Evolution Result.

Это обеспечивает:

- воспроизводимость;
- аудит;
- изменение алгоритмов без потери истории.

---

# Long-term Vision

Evolution Result является универсальным контрактом между всеми Engine платформы.

Благодаря этому Footforall может бесконечно расширять систему Evolution Engine, не изменяя архитектуру Player Profile.
