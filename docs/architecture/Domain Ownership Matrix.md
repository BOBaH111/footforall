# 16 Domain Ownership Matrix

# Purpose

This document defines ownership and lifecycle responsibilities for every domain object.

It specifies:

- Aggregate ownership
- Creation rules
- Update rules
- Immutability rules
- Source of truth

The matrix is the foundation for:

- Database design
- Repository implementation
- REST API
- Permission model
- Domain services

---

| Domain Object | Aggregate Owner | Created By | Updated By | Immutable After | Source of Truth |
|---------------|-----------------|------------|------------|-----------------|-----------------|
| User | User | Registration | User / System | Never | User |
| Player Role | User | User | User | Never | User |
| Referee Role | User | User | User | Never | User |
| Venue Manager Role | User | User | User | Never | User |
| Venue | Venue | Venue Manager | Venue Manager | Archived | Venue |
| Match | Match | Organizer | Organizer / System | Protocol Published | Match |
| Registration | Match | Player | Player / System | Match Start | Match |
| Team | Match | System | System | Match Start | Match |
| Participation | Match | System | System | Match Finish | Match |
| Match Event | Match | Referee | Referee* | Protocol Submitted | Match |
| Match Protocol | Match Protocol | System | Assigned Referee | Published | Match Protocol |
| Statistics | Projection | System | System | Recalculated | Published Protocol |
| Skill | Projection | System | System | Recalculated | Published Protocol |
| Reputation | Projection | System | System | Recalculated | Published Protocol |
| Overall Score | Projection | System | System | Recalculated | Published Protocol |
| Achievements | Projection | System | System | Recalculated | Published Protocol |
| Payment | Payment | System | System | Completed | Payment |
| Notification | Notification | System | System | Delivered | Notification |

---

# Notes

(*) Referee may only edit events until the protocol is submitted.

After protocol submission all recorded events become immutable.

---

# Source of Truth

Only the following Aggregates own business data.

- User
- Venue
- Match
- Match Protocol
- Payment
- Notification

Everything else is derived.

---

# Projection Model

Statistics

↓

Skill

↓

Reputation

↓

Overall Score

↓

Achievements

are generated automatically.

No projection is edited manually.

---

# Data Preservation Principle

Historical football data is never deleted.

Objects become archived instead.

Match history is immutable.

Protocol history is immutable.

Football Identity is reproducible from published protocols.
