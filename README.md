# Peer Backend Portfolio — Peer Network (Merged PR Highlights)

This repository is a curated **engineering portfolio** built from my merged pull requests in the open-source **Peer Network** backend.
Each highlight follows a consistent format: **Problem → Solution → Tech → Outcome**, with direct links to the original PRs for verification.

> Primary upstream repository: **peer-network/peer_backend** (public)

## Portfolio Highlights
1. Personalized Feed Ranking & Social Graph (B)
2. Moderation & Reporting Revamp (C)
3. Tokenomics & Fee Model Migration (D)
4. Media Pipeline: Auto Video Covers + Illegal Content Hardening (E)
5. Backend Code Quality Program (A)

---

## 1) Personalized Feed Ranking & Social Graph (B)

### Problem
- A generic feed wasn’t aligned with how users interact (friends/follows).
- Follow-state flags were ambiguous (`isfollowed` vs `isfollowing`), causing frontend confusion.
- Tag-based discovery was unreliable due to mixed casing persisted in DB.

### Solution
- Implemented a personalized “For Me” feed ordering using social graph signals.
- Introduced explicit follow-direction flags while keeping legacy fields for backward compatibility.
- Normalized tags to lowercase and fixed search behavior, including DB cleanup/migration.

### Implementation Highlights
- Ranking/ordering logic in feed query + filter behavior adjustments.
- Added `iFollowThisUser` and `thisUserFollowsMe` flags across profile responses and GraphQL schema.
- Tag normalization + DB migration to remove duplicates and enforce lowercase.

### Verified PRs
- Main: https://github.com/peer-network/peer_backend/pull/347
- Supporting:
  - https://github.com/peer-network/peer_backend/pull/495
  - https://github.com/peer-network/peer_backend/pull/697
  - https://github.com/peer-network/peer_backend/pull/791
  - https://github.com/peer-network/peer_backend/pull/560
  - https://github.com/peer-network/peer_backend/pull/783

### Tech
PHP, GraphQL, SQL, pagination/filters, backwards compatibility, DB migration

### Outcome
- More relevant feed behavior driven by social graph.
- Clear follow relationship semantics for UI states.
- Reliable tag-based search and normalized data.

---

## 2) Moderation & Reporting Revamp (C)

### Problem
- Reports were stored across multiple legacy `*_reports` tables, creating operational “noise” and inconsistent querying.
- Moderation payloads lacked essential context (who authored the content; who moderated the ticket).

### Solution
- Unified reports into a single `user_reports` table and updated code queries/mappings.
- Enriched moderationItems target content with author information for posts and comments.
- Added `moderatedBy` field to make moderation actions auditable.
- Added `isReported` flag where frontend needed it.

### Verified PRs
- Main: https://github.com/peer-network/peer_backend/pull/710
- Supporting:
  - https://github.com/peer-network/peer_backend/pull/789
  - https://github.com/peer-network/peer_backend/pull/796
  - https://github.com/peer-network/peer_backend/pull/800
  - https://github.com/peer-network/peer_backend/pull/706

### Tech
SQL migrations, API response contracts, DTO/mappers, moderation workflows

### Outcome
- Cleaner data model for reporting.
- Better moderation tooling payloads (author + moderator attribution).
- Reduced ambiguity for QA and operations when reviewing tickets.

---

## 3) Tokenomics & Fee Model Migration (D)

### Problem
- Users needed transparency: how much each in-app action costs (tokens mapped to EUR).
- Liquidity pool fee was deprecated; removing it naïvely would break accounting totals per operation.
- Token transfer flow needed centralized validation (min amount, message validation, decimals) and more stable numeric formatting.

### Solution
- Implemented `getActionPrice` API + DB table for action pricing (initially hardcoded by product decision at that time).
- Removed LP fee from the fee model and migrated historical transactions while preserving per-operation totals.
- Added validation/constants for transfer message + minimum amount; fixed scientific notation responses and decimal constraints.
- Implemented onboarding shown-state + preferences and added tokenomics response elements.

### Verified PRs
- Main: https://github.com/peer-network/peer_backend/pull/226
- Supporting:
  - https://github.com/peer-network/peer_backend/pull/718
  - https://github.com/peer-network/peer_backend/pull/763
  - https://github.com/peer-network/peer_backend/pull/734
  - https://github.com/peer-network/peer_backend/pull/811
  - https://github.com/peer-network/peer_backend/pull/528

### Tech
SQL scripts/migrations, token accounting, validation, constants/config, GraphQL, JSONB

### Outcome
- Action pricing visibility for users.
- Fee model evolution without breaking historical accounting.
- Safer, more predictable transfer behavior and responses.

---

## 4) Media Pipeline: Auto Video Covers + Illegal Content Hardening (E)

### Problem
- Video posts without explicit cover images looked inconsistent.
- After moderation marked posts as illegal, covers could still leak via normal media paths in API responses.

### Solution
- Added automatic cover image generation for video posts:
  - Extract a frame via FFmpeg at 1 second → save as JPG → upload via base64 handler → cleanup temp file.
- Extended moderation handling to move cover files into `/media/illegal` alongside main media and wire covers into placeholder/filtering logic.

### Verified PRs
- Main: https://github.com/peer-network/peer_backend/pull/301
- Supporting:
  - https://github.com/peer-network/peer_backend/pull/749
  - https://github.com/peer-network/peer_backend/pull/760

### Tech
FFmpeg, backend services, file handling, media storage, content filtering

### Outcome
- Automatic video covers for better UX.
- No more cover exposure for illegal content through API responses.

---

## 5) Backend Code Quality Program (A)

### Problem
- High static-analysis debt (PHPStan findings) and inconsistent type/validation rules across services.
- Lack of consistent modernization rules for refactoring at scale.

### Solution
- Large-scale refactor to reduce PHPStan findings and raise strictness.
- Rolled out Rector rules for consistent modernization.
- Consolidated shared constants/config to prevent mismatched validation rules across services.

### Verified PRs
- Main: https://github.com/peer-network/peer_backend/pull/360
- Supporting:
  - https://github.com/peer-network/peer_backend/pull/373
  - https://github.com/peer-network/peer_backend/pull/585
  - https://github.com/peer-network/peer_backend/pull/574
  - https://github.com/peer-network/peer_backend/pull/770
  - https://github.com/peer-network/peer_backend/pull/771
  - https://github.com/peer-network/peer_backend/pull/772
  - https://github.com/peer-network/peer_backend/pull/773
  - https://github.com/peer-network/peer_backend/pull/774

### Tech
PHPStan, Rector, strict_types, refactoring, codebase standardization

### Outcome
- Cleaner, more maintainable codebase with consistent rules.
- Safer refactoring and better long-term delivery velocity.

---

## Interview Talking Points (quick)
- I ship product features (feed ranking, action pricing, onboarding).
- I evolve data models safely (reporting unification, fee model migrations).
- I harden platform behavior (media covers + illegal content handling).
- I reduce technical debt at scale (PHPStan + Rector + strict types + constants consolidation).

