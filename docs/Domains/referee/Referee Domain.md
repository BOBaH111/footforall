# 09 Referee Domain

# Purpose

The Referee role is responsible for ensuring an objective and structured football match.

Unlike players, referees do not participate in the game itself.
Their primary responsibility is to manage the match according to the rules and produce an immutable match protocol.

The quality of refereeing is measured exclusively by reputation earned through completed matches.

---

# Product Principles

- Referee is a role of a User.
- One User may simultaneously be Player, Referee and Venue Manager.
- Referees never create matches.
- Referees independently apply for published matches.
- Players choose the preferred referee.
- Only one referee is assigned to a match.
- Only the assigned referee may manage the match protocol.
- After protocol approval, match events become immutable.
- Referee reputation is based only on completed matches and player evaluations.

---

# Referee Responsibilities

Before the match:

- Complete referee onboarding.
- Confirm possession of required equipment.
- Browse available matches.
- Apply for referee positions.

During the match:

- Check in players using QR codes.
- Start the match.
- Record all match events.
- Manage match timeline.
- Finish the match.
- Submit the protocol.

After the match:

- Publish the protocol.
- Receive player ratings.
- Participate in dispute resolution if required.

---

# Referee Equipment

Every referee must confirm possession of:

- Black referee uniform
- Whistle
- Yellow card
- Red card

The platform does not verify equipment physically.
The referee confirms readiness during onboarding.

---

# Referee Profile

A referee profile extends the User role.

Contains:

- Biography
- Spoken languages
- Equipment confirmation
- Reputation
- Statistics

No licenses or qualification levels are stored.

Experience is represented only by completed matches and reputation.

---

# Referee Application

A referee must submit an application before participating in a match.

Multiple referees may apply for the same match.

Players choose their preferred referee before registration closes.

The system automatically assigns the referee with the highest support.

---

# Referee Application Status

- APPLIED
- SELECTED
- DECLINED
- CANCELLED
- COMPLETED

---

# Referee Match Lifecycle

AVAILABLE

↓

APPLIED

↓

SELECTED

↓

CHECKED_IN

↓

OFFICIATING

↓

COMPLETED

---

# Match Responsibilities

Only the assigned referee may:

- Check in players
- Start the match
- Record events
- Pause the match
- Resume the match
- Finish the match
- Submit protocol

---

# Match Protocol

The referee prepares the official match protocol.

The protocol contains:

- Timeline
- Goals
- Cards
- Fouls
- Own goals
- Penalties
- Player substitutions
- Match result

After approval the protocol becomes immutable.

---

# Ratings

Players evaluate the referee after protocol publication.

Ratings affect:

- Reputation
- Future match selection probability

Ratings never directly modify match events.

---

# Disputes

Players may open a dispute after protocol publication.

A dispute never changes recorded events directly.

Disputes follow a separate moderation workflow.

---

# Statistics

Statistics are generated automatically.

Examples:

- Matches officiated
- Matches completed
- Matches cancelled
- Yellow cards shown
- Red cards shown
- Average player rating
- Disputes opened
- Disputes confirmed

Statistics cannot be edited manually.

---

# Business Rules

## BR-REF-001

Only Users with the Referee role may apply to referee matches.

---

## BR-REF-002

Multiple referee applications are allowed for one match.

---

## BR-REF-003

Players may choose a preferred referee before registration closes.

---

## BR-REF-004

The platform assigns exactly one referee.

---

## BR-REF-005

Only the assigned referee may manage the match.

---

## BR-REF-006

Only the assigned referee may submit the protocol.

---

## BR-REF-007

After protocol approval, all recorded events become immutable.

---

## BR-REF-008

Player ratings are available only after protocol publication.

---

## BR-REF-009

Disputes do not modify the protocol directly.
