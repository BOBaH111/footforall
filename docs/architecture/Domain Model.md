# 08. Domain Model

## Purpose

This document describes the core domain model of Footforall.

The system is designed according to Domain-Driven Design (DDD), where every business concept has a clear responsibility and lifecycle.

---

# Core Principles

Footforall is **not** a football field booking service.

Footforall is a platform that builds the digital football reputation of every participant in the ecosystem.

Every completed match creates new digital value.

---

# Main Domains

```text
Users
Matches
Venues
Payments
Notifications
Ratings
```

Each domain is independent and communicates only through well-defined business contracts.

---

# User

A User represents an account in the system.

The User itself has no football statistics.

It stores:

- authentication
- contact information
- settings
- permissions
- available roles

```text
User
```

---

# User Roles

A user may have multiple independent roles.

```text
User
│
├── PlayerProfile
├── RefereeProfile
└── VenueManagerProfile
```

Each role has its own lifecycle.

Each role evolves independently.

---

# Player

Represents a person's football career.

Contains:

- Statistics
- Rating
- Reputation
- Match History

---

# Referee

Represents a person's referee career.

Contains:

- Statistics
- Rating
- Reputation
- Match History

---

# Venue Manager

Represents a person or company managing football venues.

Contains:

- managed venues
- operator reputation
- operational statistics
- service reliability

A Venue Manager is **not** a football venue.

---

# Venue

Venue is an independent business aggregate.

A venue is a digital representation of a football field.

```text
Venue
```

A venue owns its own:

- Statistics
- Rating
- Reputation

A venue exists independently from its manager.

Changing the manager does not reset venue history.

---

# Ownership

```text
VenueManager

1

↓

N

Venue
```

One manager may operate many venues.

Each venue has one active manager.

Management history may be stored separately.

---

# Match

Match is the central aggregate of the system.

Every completed match produces objective evidence.

```text
Match
        │
        ▼
Evidence
```

---

# Evolution Model

Every participant evolves after a completed match.

```text
Match
      │
      ▼
Evidence
      │
      ▼
Evolution Pipeline
      │
 ┌────┼──────────────┐
 ▼    ▼              ▼
Statistics Rating Reputation
      │
      ▼
 Aggregate
```

---

# Evolution Contract

Every evolving aggregate must implement the same architecture.

## Aggregate

Stores domain state.

## Evidence

Immutable snapshot produced by one completed match.

## Statistics Engine

Calculates new statistics.

## Rating Engine

Calculates rating.

## Reputation Engine

Calculates reputation.

## Evolution Pipeline

Coordinates the evolution process.

---

# Common Architecture

Every evolving aggregate contains:

```text
Status
Statistics
Rating
Reputation
Evidence
Statistics Engine
Rating Engine
Reputation Engine
Evolution Pipeline
```

---

# Reputation

Reputation is independent from Rating.

Rating answers:

> How strong is this participant?

Reputation answers:

> How trustworthy is this participant?

Reputation changes slower than Rating and is based on long-term behaviour.

---

# Digital Football Identity

Every completed match updates the participant's digital football identity.

```text
Statistics

+

Rating

+

Reputation

↓

Digital Football Identity
```

This principle applies to:

- Player
- Referee
- Venue
- Venue Manager
- future roles (Coach, Club, Scout, etc.)

---

# Scalability

The architecture is designed for unlimited extension.

New participant types can be added without changing existing domains.

Examples:

- Coach
- Club
- Scout
- Academy
- Tournament
- League

All follow the same Evolution Contract.

---

# North Star

Every completed match increases the value of the ecosystem.

The match is the source of truth.

Digital reputation is the main product of Footforall.
