# Footforall — API Design

Version: 1.0  
Status: Draft  
Last Updated: 2026-07-06  

---

# 1. Общие принципы API

## API-PRINCIPLE-001. REST-first архитектура

Система использует REST API поверх HTTP.

---

## API-PRINCIPLE-002. Resource-oriented design

Все эндпоинты строятся вокруг сущностей:

- users
- matches
- venues
- registrations
- events
- payments
- ratings
- protocols

---

## API-PRINCIPLE-003. Match-centric model

Матч является центральным ресурсом API.

---

## API-PRINCIPLE-004. Без бизнес-логики в контроллерах

Контроллеры не содержат логики:

- вся логика в сервисах (NestJS Services)
- API только транслирует действия в доменный слой

---

# 2. Authentication API

---

## POST /auth/register

Создание пользователя.

### Request
```json
{
  "name": "string",
  "email": "string",
  "phone": "string",
  "password": "string"
}
```

### Response
```json
{
  "user_id": "uuid",
  "status": "created"
}
```

---

## POST /auth/login

Авторизация пользователя.

---

# 3. Users API

---

## GET /users/:id

Получение профиля пользователя.

---

## GET /users/:id/stats

Статистика игрока:

- матчи
- голы
- карточки
- рейтинг

---

## PATCH /users/:id/block

Блокировка пользователя (admin only)

---

# 4. Venues API

---

## POST /venues

Создание площадки.

### Business Rule: BR-VENUE-001

---

## GET /venues/:id

Получение информации о площадке.

---

## GET /venues/:id/rating

Рейтинг площадки.

---

# 5. Matches API

---

## POST /matches

Создание матча.

### Linked BR:
- BR-MATCH-001

---

## GET /matches/:id

Детали матча.

---

## PATCH /matches/:id/publish

Публикация матча.

### Linked BR:
- BR-MATCH-003

---

## PATCH /matches/:id/cancel

Отмена матча.

---

## GET /matches

Список матчей с фильтрами:

- venue_id
- status
- date
- format

---

# 6. Registrations API

---

## POST /matches/:id/register

Регистрация игрока на матч.

### Linked BR:
- BR-MATCH-004

### Request
```json
{
  "role": "goalkeeper | defender | midfielder | forward"
}
```

---

## PATCH /matches/:id/unregister

Отмена регистрации.

---

## GET /matches/:id/registrations

Список участников матча.

---

# 7. Teams API

---

## POST /matches/:id/teams/auto

Автоматическое распределение команд.

### Linked BR:
- BR-MATCH-006 (casual mode)

---

## POST /matches/:id/teams/manual

Ручное формирование команд (tournament mode)

---

## GET /matches/:id/teams

Получение команд матча.

---

# 8. Match Events API

---

## POST /matches/:id/events

Создание игрового события (рефери)

### Linked BR:
- BR-EVENT-002

### Request
```json
{
  "type": "goal | own_goal | penalty | yellow_card | red_card | substitution | match_start | half_time | match_end",
  "minute": 35,
  "data": {}
}
```

---

## GET /matches/:id/events

История событий матча.

---

# 9. Protocol API

---

## POST /matches/:id/protocol/finalize

Финализация протокола (referee only)

### Linked BR:
- BR-PROTOCOL-001
- BR-PROTOCOL-002

---

## GET /matches/:id/protocol

Получение итогового протокола.

---

# 10. Payments API

---

## POST /payments/match-fee

Оплата участия игрока.

---

## GET /users/:id/payments

История платежей пользователя.

---

## POST /payments/refund

Возврат средств (до начала матча)

### Linked BR:
- BR-PAYMENT-003

---

# 11. Ratings API

---

## POST /matches/:id/ratings

Создание оценки после матча.

### Types:
- player → player rating
- referee → referee rating
- venue → venue rating

---

## GET /users/:id/ratings

Получение рейтингов пользователя.

---

# 12. System Rules for API

---

## API-RULE-001. Immutable events

Match events cannot be updated via API after match completion.

---

## API-RULE-002. Role-based access control

- Player → register, rate
- Referee → manage events, protocol
- Venue owner → manage matches
- Admin → full access

---

## API-RULE-003. Match state gating

All endpoints are gated by match status:

Example:
- register → only if registration_open
- events → only if match_started
- protocol → only if finished

---

# 13. Future extensions

- tournaments API
- analytics API
- player career API
- referee assignment system
- AI match insights API

---

# 14. Core philosophy

API is not a CRUD layer.

API is a controlled interface to the football match lifecycle engine.
