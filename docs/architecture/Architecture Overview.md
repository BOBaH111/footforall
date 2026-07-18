# 12 Architecture Overview

# Purpose

This document defines the high-level architecture of Footforall.

It describes:

- Aggregate boundaries
- Domain ownership
- Source of truth
- Data flow
- Domain services
- Projection model

This document is the foundation for the database model, REST API and backend implementation.

---

# Architecture Principles

Footforall follows Domain-Driven Design (DDD).

Business logic is designed before infrastructure.

Every business capability belongs to exactly one Aggregate.

Aggregates communicate through identifiers and domain services.

No Aggregate directly modifies another Aggregate.

---

# Core Aggregate Roots

The platform consists of the following Aggregate Roots.

## User

Represents a platform account.

Responsible for:

- authentication
- profile
- roles
- permissions

---

## Player

Represents the football career of a user.

Responsible for:

- football profile
- career
- football identity

---

## Referee

Represents referee activity.

Responsible for:

- referee profile
- referee applications
- officiating history

---

## Venue

Represents a football venue.

Responsible for:

- venue profile
- availability
- infrastructure

---

## Match

Central Aggregate of the platform.

Responsible for:

- match lifecycle
- registrations
- teams
- check-in
- gameplay state

---

## Match Protocol

Official record of a completed match.

Responsible for:

- protocol lifecycle
- event confirmation
- immutable match history

---

# Source of Truth

The platform has only four primary sources of truth.

- User
- Venue
- Match
- Match Protocol

Everything else is derived automatically.

---

# Derived Data

The following data is calculated.

- Statistics
- Skill
- Reputation
- Overall Score
- Achievements

Derived data may always be rebuilt from published protocols.

---

# Match Lifecycle

Draft

↓

Published

↓

Registration

↓

Team Formation

↓

Check-in

↓

Match

↓

Finished

↓

Protocol Submission

↓

Protocol Publication

↓

Match Finalization

---

# Match Finalization

Protocol publication starts the Match Finalization process.

It updates the entire ecosystem.

---

# Domain Calculation Engines

The platform contains several independent calculation engines.

## Statistics Engine

Updates football statistics.

---

## Skill Engine

Calculates football ability.

---

## Reputation Engine

Calculates participant reliability.

---

## Overall Score Engine

Combines Skill and Reputation.

---

## Achievement Engine

Awards achievements.

---

## Payment Engine

Processes financial settlement.

---

## Notification Engine

Sends platform notifications.

---

# Data Flow

Published Match Protocol

↓

Statistics Engine

Skill Engine

Reputation Engine

↓

Overall Score Engine

↓

Achievement Engine

↓

Football Identity

---

# Football Identity

Every participant has one Football Identity.

Football Identity contains:

- Career
- Statistics
- Skill
- Reputation
- Overall Score
- Achievements

Football Identity is updated automatically.

---

# Aggregate Communication

Aggregates never modify each other directly.

Communication is performed through:

- identifiers
- domain services

Example:

Match

↓

Match Protocol

↓

Statistics Engine

↓

Player

---

# Projection Model

Statistics

Ratings

Achievements

Football Identity

are projections.

They are not the primary source of truth.

They can always be rebuilt.

---

# Architecture Goals

The architecture guarantees:

- immutable match history
- objective rating calculation
- isolated business domains
- scalable services
- extensibility
- deterministic calculations
