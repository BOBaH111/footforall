# 08. Infrastructure Principles

## Purpose

This document defines the architectural principles of the Infrastructure layer in Footforall.

The Infrastructure layer is responsible only for persistence and communication with external systems.

It must never contain business rules.

---

# General Principles

## 1. Domain First

The Domain layer is the source of truth.

Infrastructure exists only to store and retrieve domain objects.

Business rules must never depend on Prisma, PostgreSQL or any framework.

---

## 2. One Aggregate → One Table

Each Aggregate Root is stored in a single database table.

Value Objects are flattened into columns.

Example:

Venue
 ├── VenueProfile
 ├── VenueLocation
 ├── VenueInfrastructure
 ├── VenueStatistics
 ├── VenueRating
 └── VenueReviewSummary

↓

Venue table

Separate tables for Value Objects are not allowed.

---

## 3. Prisma mirrors the Domain

The Prisma Schema reflects the Domain model.

The Domain must never be modified to satisfy Prisma limitations.

If a compromise is required, it must happen inside Mapper classes.

---

## 4. Repository restores Aggregates

Repositories never construct domain objects manually.

Repositories always restore aggregates through:

Venue.restore(...)

instead of

Venue.create(...)

create() is used only for new business objects.

restore() is used only when loading persisted data.

---

## 5. Mapper is the translation layer

Infrastructure communicates with the Domain only through Mappers.

Prisma Model
        ↓
   VenueMapper
        ↓
Domain Aggregate

The Domain never knows Prisma exists.

---

## 6. Stable Naming Convention

All tables use the same naming rules.

Primary key:

id

Foreign keys:

managerId
playerId
venueId
matchId

Audit fields:

createdAt
updatedAt

Status fields:

status

---

## 7. Infrastructure contains no Business Logic

Infrastructure may:

- save data
- update data
- query data
- map data

Infrastructure must never:

- validate business rules
- calculate ratings
- change match state
- create teams
- perform business decisions

---

## 8. Persistence Independence

The Domain must be replaceable without changing business logic.

Future migrations:

Prisma → TypeORM

PostgreSQL → MySQL

SQL → MongoDB

must require changes only inside Infrastructure.

---

## 9. MVP First

Infrastructure is implemented only when required by the MVP.

Premature optimization is avoided.

Examples of postponed functionality:

- caching
- CQRS
- Event Sourcing
- Read Models
- Microservices
- Message Brokers

These may be introduced only after a proven business need.

---

## 10. Infrastructure Goal

Infrastructure should be simple,
predictable,
testable,
and invisible to the business logic.

The Domain must remain the heart of Footforall.
