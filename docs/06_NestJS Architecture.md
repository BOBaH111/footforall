# Footforall — NestJS Architecture

Version: 1.0  
Status: Draft  
Last Updated: 2026-07-06  

---

# 1. Общая архитектурная идея

Backend Footforall строится по принципам:

- Domain-Driven Design (DDD light)
- Modular Monolith (на первом этапе)
- Event-driven внутри системы
- Чёткое разделение bounded contexts

---

# 2. Общая структура backend

```
src/
 ├── modules/
 ├── shared/
 ├── infrastructure/
 ├── database/
 └── main.ts
```

---

# 3. Bounded Contexts (основные домены)

Система делится на независимые доменные модули:

1. Users
2. Matches
3. Venues
4. Registrations
5. Teams
6. Events
7. Protocols
8. Payments
9. Ratings
10. Notifications

---

# 4. Модули NestJS

---

## 4.1 Users Module

### Ответственность:
- регистрация
- авторизация
- роли
- блокировки
- профиль пользователя

### Структура:
```
users/
 ├── users.controller.ts
 ├── users.service.ts
 ├── users.repository.ts
 ├── dto/
 └── entities/
```

---

## 4.2 Matches Module

### Центральный модуль системы

### Ответственность:
- создание матча
- управление жизненным циклом
- статусная машина (Match Engine)
- публикация / отмена

---

## 4.3 Venues Module

### Ответственность:
- создание площадок
- управление площадками
- рейтинг площадок

---

## 4.4 Registrations Module

### Ответственность:
- регистрация игроков на матч
- выбор амплуа (match role)
- контроль слотов
- управление оплатой регистрации

---

## 4.5 Teams Module

### Ответственность:
- автоматическое распределение игроков
- ручное формирование команд
- фиксация состава

---

## 4.6 Events Module

### Ответственность:
- фиксация игровых событий
- валидация событий по статусу матча
- хранение event log

---

## 4.7 Protocols Module

### Ответственность:
- формирование итогового протокола
- финализация матча
- фиксация результата

---

## 4.8 Payments Module

### Ответственность:
- оплата участия
- выплаты судье
- выплаты площадке
- возвраты
- комиссия платформы

---

## 4.9 Ratings Module

### Ответственность:
- расчёт рейтинга игроков
- рейтинг судей
- рейтинг площадок
- агрегация после завершения матча

---

## 4.10 Notifications Module

### Ответственность:
- push уведомления
- email уведомления
- SMS уведомления
- события матча (match lifecycle events)

---

# 5. Shared Layer

---

## shared/

Общие компоненты:

- guards (auth, roles)
- interceptors
- decorators
- enums
- utils
- constants

---

# 6. Infrastructure Layer

---

## infrastructure/

### Содержит:

- database connection (PostgreSQL / Prisma / TypeORM)
- external services (payments, SMS, email)
- caching (Redis)
- queue system (BullMQ)

---

# 7. Match Module — ядро системы

---

## Внутренняя структура:

```
matches/
 ├── matches.controller.ts
 ├── matches.service.ts
 ├── match-state-machine.service.ts
 ├── match-lifecycle.guard.ts
 ├── dto/
 ├── entities/
 └── events/
```

---

## Ключевой компонент:

### match-state-machine.service.ts

Отвечает за:

- переходы статусов
- валидацию переходов
- триггеры событий
- защиту от нелегальных состояний

---

# 8. Event-driven внутри backend

---

## Принцип:

Каждое важное действие генерирует событие:

Примеры:

- MatchPublished
- PlayerRegistered
- MatchStarted
- GoalScored
- MatchCompleted

---

## Использование:

События используются для:

- рейтинга
- статистики
- платежей
- уведомлений

---

# 9. Data flow (упрощённо)

```
Controller
   ↓
Service (business logic)
   ↓
State Machine / Domain logic
   ↓
Repository (DB)
   ↓
Event Dispatcher
   ↓
Other modules (ratings, payments, notifications)
```

---

# 10. Ключевые архитектурные правила

---

## ARC-001. Match-first design

Любая логика должна быть привязана к матчу.

---

## ARC-002. No cross-module logic

Модули не должны напрямую вызывать внутреннюю логику друг друга.

Используются:

- события
- сервисы интерфейсов

---

## ARC-003. State machine ownership

Только Matches Module управляет:

- статусами матча
- переходами состояний

---

## ARC-004. Events are immutable

События:

- не редактируются
- только добавляются

---

## ARC-005. Payments isolation

Payments module не зависит от бизнес-логики матча напрямую.

---

# 11. Будущие расширения

---

## Phase 2 modules:

- Tournaments Module
- Player Career Module
- AI Analytics Module
- Referee Assignment Engine
- Anti-cheat System

---

# 12. Итоговая архитектурная идея

Footforall backend =

> Modular monolith + event-driven domain system вокруг матча
