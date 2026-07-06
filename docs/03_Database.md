# Footforall — Database Design

Version: 1.0  
Status: Draft  
Last Updated: 2026-07-06  

---

# 1. Общая концепция

База данных Footforall строится вокруг событийной модели:

- Match = центральная сущность
- всё остальное — производные или связанные сущности
- все изменения фиксируются через события и состояния

Используется PostgreSQL.

---

# 2. Основные принципы моделирования

## DB-PRINCIPLE-001. Централизация вокруг Match

Все ключевые сущности привязаны к матчу:

- registrations
- match_events
- protocols
- payments
- ratings

---

## DB-PRINCIPLE-002. Историчность данных

- события не изменяются после фиксации
- результаты матчей сохраняются навсегда
- корректировки создаются как новые записи

---

## DB-PRINCIPLE-003. Разделение сущностей и событий

- сущности = Users, Matches, Venues
- события = MatchEvents, Payments, Ratings

---

# 3. Основные сущности

---

# 3.1 Users

```sql
users
-----
id UUID PK
name TEXT
email TEXT UNIQUE
phone TEXT UNIQUE
password_hash TEXT
created_at TIMESTAMP
status ENUM(active, blocked, deleted)
```

---

# 3.2 User Roles

Пользователь может иметь несколько ролей.

```sql
user_roles
----------
id UUID PK
user_id UUID FK -> users.id
role ENUM(player, referee, venue_owner, admin)
```

---

# 3.3 Venues (Площадки)

```sql
venues
-------
id UUID PK
owner_id UUID FK -> users.id
name TEXT
address TEXT
capacity INT
rating NUMERIC(3,2)
status ENUM(active, archived)
created_at TIMESTAMP
```

---

# 3.4 Matches

```sql
matches
-------
id UUID PK
venue_id UUID FK -> venues.id
referee_id UUID FK -> users.id

date DATE
start_time TIME
duration INT

format ENUM(5v5, 6v6, 7v7, 8v8, 11v11)

price_per_player NUMERIC

status ENUM(
draft,
published,
registration_open,
registration_closed,
teams_forming,
check_in,
ready,
first_half,
half_time,
second_half,
finished,
protocol_pending,
completed,
cancelled
)

created_at TIMESTAMP
```

---

# 3.5 Match Registrations

```sql
match_registrations
-------------------
id UUID PK
match_id UUID FK -> matches.id
user_id UUID FK -> users.id

role ENUM(goalkeeper, defender, midfielder, forward)

payment_status ENUM(pending, paid, refunded)

status ENUM(registered, cancelled, confirmed)

created_at TIMESTAMP
```

---

# 3.6 Match Teams

```sql
match_teams
-----------
id UUID PK
match_id UUID FK -> matches.id
name TEXT (Team A / Team B)
```

---

```sql
match_team_players
------------------
id UUID PK
team_id UUID FK -> match_teams.id
user_id UUID FK -> users.id
```

---

# 3.7 Match Events

```sql
match_events
------------
id UUID PK
match_id UUID FK -> matches.id
user_id UUID FK -> users.id

type ENUM(
goal,
own_goal,
penalty,
yellow_card,
red_card,
substitution,
match_start,
half_time,
match_end
)

minute INT
data JSONB
created_at TIMESTAMP
```

---

# 3.8 Protocols

```sql
match_protocols
---------------
id UUID PK
match_id UUID FK -> matches.id
referee_id UUID FK -> users.id

final_score_home INT
final_score_away INT

status ENUM(draft, confirmed, finalized)

created_at TIMESTAMP
```

---

# 3.9 Payments

```sql
payments
--------
id UUID PK
user_id UUID FK -> users.id
match_id UUID FK -> matches.id

type ENUM(player_fee, referee_fee, venue_fee, platform_fee)

amount NUMERIC
status ENUM(pending, completed, failed, refunded)

created_at TIMESTAMP
```

---

# 3.10 Ratings

```sql
ratings
-------
id UUID PK
match_id UUID FK -> matches.id
from_user_id UUID FK -> users.id
to_user_id UUID FK -> users.id

type ENUM(player, referee, venue)

score INT
comment TEXT

created_at TIMESTAMP
```

---

# 4. Связи между сущностями

- User → Matches (via registrations)
- Match → Venue
- Match → Referee (User)
- Match → Events
- Match → Protocol
- Match → Payments
- Users → Ratings (bidirectional)

---

# 5. Индексация (важно для масштаба)

## Рекомендуемые индексы:

- match_id во всех таблицах
- user_id во всех таблицах
- (match_id, user_id) в registrations
- status в matches
- created_at во всех таблицах событий

---

# 6. Архитектурные замечания

## ⚠️ 1. JSONB в match_events

Используется для:

- дополнительной информации события
- гибкости расширения типов событий

---

## ⚠️ 2. Рейтинг не агрегируется в таблице users

Рейтинг считается:

- либо через материализованные представления
- либо через сервисный слой

---

## ⚠️ 3. Payments отделены от Match logic

Финансы не влияют на lifecycle матча напрямую

---

# 7. Будущие расширения

- tournament tables
- seasonal stats
- player career history
- AI analytics layer
- anti-cheat system

---

# 8. Принцип базы данных

> База данных должна отражать реальный футбольный матч как цифровую модель, а не как набор абстрактных таблиц.
