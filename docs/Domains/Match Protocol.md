# 10 Match Protocol

# Purpose

The Match Protocol is the official record of a completed football match.

It summarizes all recorded match events and becomes the immutable source for statistics, ratings, achievements and historical records.

The protocol is generated automatically during the match as the referee records events.

---

# Product Principles

- Every completed match has exactly one protocol.
- The protocol is generated automatically from recorded match events.
- Referees do not rewrite the protocol after the match.
- The referee only reviews and submits the generated draft.
- After submission all match events become immutable.
- Statistics are calculated only from the published protocol.
- The protocol is the only official source of match history.

---

# Draft Protocol

During the match every recorded event immediately updates the draft protocol.

Examples:

- Kick-off
- Goals
- Yellow cards
- Red cards
- Penalties
- Own goals
- Substitutions
- Half-time
- Second half
- Full-time

The referee always sees the current draft protocol.

---

# Referee Responsibilities

The assigned referee may:

- Review the draft protocol.
- Add an optional match comment.
- Submit the protocol.

The referee cannot manually create, remove or rewrite recorded match events.

---

# Protocol Lifecycle

DRAFT

↓

SUBMITTED

↓

PUBLISHED

↓

DISPUTED

↓

CLOSED

---

# Status Description

## DRAFT

Protocol is automatically updated during the match.

---

## SUBMITTED

The referee confirms that the protocol is complete.

All recorded events immediately become immutable.

---

## PUBLISHED

Players can view the official protocol.

Players can rate the referee.

Players can open disputes.

---

## DISPUTED

At least one dispute has been opened.

The protocol itself remains unchanged.

---

## CLOSED

Disputes are resolved.

Ratings, statistics and achievements become final.

---

# Match Events

The protocol is generated from recorded match events.

Examples:

- Kick-off
- Goal
- Own Goal
- Yellow Card
- Red Card
- Penalty Awarded
- Penalty Scored
- Penalty Missed
- Substitution
- Half-time
- Second Half
- Full-time

---

# Immutable History

After protocol submission:

- Match events cannot be edited.
- Match events cannot be removed.
- Match events cannot be reordered.

The historical record is immutable.

---

# Ratings

After publication players may:

- Rate the referee.
- Open disputes.

Ratings never modify the protocol.

---

# Statistics

Only published protocols update:

- Player statistics
- Referee statistics
- Venue statistics
- Rankings
- Achievements

---

# Business Rules

## BR-PROTOCOL-001

Every match has exactly one protocol.

---

## BR-PROTOCOL-002

The protocol is generated automatically from recorded match events.

---

## BR-PROTOCOL-003

Only the assigned referee may submit the protocol.

---

## BR-PROTOCOL-004

After submission all recorded events become immutable.

---

## BR-PROTOCOL-005

Only published protocols are visible to players.

---

## BR-PROTOCOL-006

Players may open disputes only after publication.

---

## BR-PROTOCOL-007

Disputes never modify the protocol directly.

---

## BR-PROTOCOL-008

Statistics are updated only from published protocols.

---

## BR-PROTOCOL-009

Match history is immutable after protocol submission.
