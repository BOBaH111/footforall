# 11 Rating & Reputation Domain

# Purpose

The Rating & Reputation Domain defines how the platform evaluates every participant.

The system does not rely on subjective opinions alone.

Every participant has three independent indicators:

- Skill
- Reputation
- Overall Score

Together they form the participant's Football Identity.

---

# Football Identity

Football Identity represents the digital football profile of a participant.

It consists of:

- Career
- Statistics
- Skill
- Reputation
- Overall Score
- Achievements

Football Identity is updated automatically after completed matches.

---

# Skill

## Purpose

Skill represents football ability.

It answers the question:

> How strong is this participant as a football player?

Skill is calculated only from objective football data.

Examples:

- Matches played
- Wins
- Goals
- Assists
- Defensive actions
- Goalkeeper saves
- Match contribution
- Consistency

Skill never depends on personal opinions.

---

# Reputation

## Purpose

Reputation represents trust inside the football community.

It answers the question:

> How reliable is this participant?

Reputation is calculated from behaviour across the platform.

Examples:

- Attendance
- Check-in completion
- Match cancellations
- No-shows
- Discipline
- Fair play
- Confirmed disputes
- Player ratings
- Referee ratings

Reputation reflects reliability rather than football ability.

---

# Overall Score

Overall Score is the participant's final public rating.

It combines:

- Skill
- Reputation

Overall Score allows participants to compare players using both football quality and community reliability.

---

# Rating Visibility

Player Profile

Overall Score

★★★★★ 4.82

Skill

★★★★★ 4.91

Reputation

★★★★☆ 4.63

---

# Principles

The platform evaluates participants using objective data.

Football ability and community behaviour are independent dimensions.

Neither Skill nor Reputation may be edited manually.

All calculations are automatic.

---

# Update Flow

Published Match Protocol

↓

Statistics Engine

↓

Skill Engine

↓

Reputation Engine

↓

Overall Score Engine

↓

Football Identity

---

# Engine Responsibilities

## Statistics Engine

Updates statistics from published protocols.

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

# Business Rules

## BR-RATING-001

Every participant has one Football Identity.

---

## BR-RATING-002

Skill is calculated only from objective football data.

---

## BR-RATING-003

Reputation is calculated from participant behaviour.

---

## BR-RATING-004

Overall Score combines Skill and Reputation.

---

## BR-RATING-005

No rating may be edited manually.

---

## BR-RATING-006

Ratings are recalculated only after protocol publication.

---

## BR-RATING-007

Historical ratings are preserved.

---

## BR-RATING-008

Football Identity is updated automatically after every completed match.

---

# Future Extensions

The architecture allows adding new dimensions without changing existing calculations.

Examples:

- Leadership
- Teamwork
- Fair Play
- MVP Score
- Seasonal Rating
- Tournament Rating
