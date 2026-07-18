# 17 Database Conventions

## Purpose

This document defines the database standards for the Footforall platform.

All database objects must follow these conventions.

---

# Database

PostgreSQL 17+

---

# Naming

## Tables

- snake_case
- plural nouns

Examples

users

venues

matches

match_registrations

participations

match_events

match_protocols

payments

notifications

---

## Columns

snake_case

Examples

created_at

updated_at

deleted_at

public_id

first_name

last_name

---

## Foreign Keys

Always

<object>_id

Examples

user_id

match_id

venue_id

team_id

participation_id

---

# Primary Keys

Every table has:

id BIGINT GENERATED ALWAYS AS IDENTITY

Never expose internal ids outside the backend.

---

# Public Identifier

Every Aggregate Root has

public_id UUID

Used only by REST API.

Example

GET /matches/{publicId}

Never expose internal numeric ids.

---

# Audit Fields

Every Aggregate Root contains

created_at

updated_at

Optionally

archived_at

No physical deletion.

---

# Delete Policy

Business entities are never deleted.

Use

status

or

archived_at

instead.

Exceptions

- OTP
- Sessions
- Cache
- Temporary imports
- Logs with retention policy

---

# Nullability

NOT NULL by default.

Nullable fields require explicit business justification.

---

# Enums

Use PostgreSQL ENUM only for stable business values.

Examples

match_status

registration_status

protocol_status

Do not use ENUM for frequently changing configurations.

---

# Monetary Values

Use

NUMERIC(12,2)

Never FLOAT.

Currency stored separately.

---

# Time

Store all timestamps

TIMESTAMP WITH TIME ZONE

UTC only.

Timezone conversion belongs to the application.

---

# Boolean Fields

Avoid multiple boolean flags.

Prefer Status Enum.

Bad

is_active

is_closed

is_cancelled

Good

status

ACTIVE

ARCHIVED

CANCELLED

---

# Indexes

Create indexes only for:

Foreign Keys

Frequently filtered fields

Frequently sorted fields

Unique constraints

Avoid unnecessary indexes.

---

# Unique Constraints

Examples

users.email

users.phone

venues.public_id

matches.public_id

---

# Check Constraints

Use database constraints where business rules are invariant.

Examples

capacity > 0

price >= 0

minute >= 0

minute <= 130

---

# Transactions

One Aggregate = One Transaction.

Cross-aggregate consistency is achieved through application services.

---

# JSON Columns

Allowed only for:

metadata

integration payloads

temporary extensibility

Core business data must be normalized.

---

# Source of Truth

Only Aggregate Roots own business data.

User

Venue

Match

Match Protocol

Payment

Notification

Everything else is either:

- Entity
- Value Object
- Projection

---

# Historical Data

Football history is immutable.

Completed matches cannot disappear.

Published protocols cannot be modified.

Statistics are reproducible from published protocols.
