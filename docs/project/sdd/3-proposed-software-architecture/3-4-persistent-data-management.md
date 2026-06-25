---
title: SDD - 3.4 Persistent Data Management | OhMyUniversity!
description: Strategy for persistent data management in OhMyUniversity!, detailing the use of PostgreSQL and Redis Cache to ensure data integrity, performance, and fault tolerance.
head:
  - - meta
    - property: og:title
      content: SDD - 3.4 Persistent Data Management | OhMyUniversity!
  - - meta
    - property: og:description
      content: Strategy for persistent data management in OhMyUniversity!, detailing the use of PostgreSQL and Redis Cache to ensure data integrity, performance, and fault tolerance.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/sdd/3-proposed-software-architecture/3-4-persistent-data-management
  - - meta
    - name: keywords
      content: ohmyuniversity, sdd, persistent data, postgresql, redis, caching, fault tolerance, agile, database, gdpr, ttl
  - - meta
    - name: twitter:title
      content: SDD - 3.4 Persistent Data Management | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Strategy for persistent data management in OhMyUniversity!, detailing the use of PostgreSQL and Redis Cache to ensure data integrity, performance, and fault tolerance.
---

# OhMyUniversity! - SDD: 3.4 Persistent Data Management

## Overview and Strategy

The persistence strategy separates three concerns: native platform data (system of record), cached
external data (disposable), and a planned document store for a feature not yet built (chat). Since
OhMyUniversity! acts as an aggregator of Esse3 data rather than a replacement for it, the system
never treats its own cache or database as the source of truth for academic records.

## 1. Relational Database (PostgreSQL) — implemented

PostgreSQL is the system of record for all data generated natively within OhMyUniversity!,
guaranteeing ACID transactions and the ability to handle concurrent access.

- **What is stored:** `OmuUser` profiles, `UniversityConnection` (linking a user to one or more
  Esse3 careers via fiscal code), calendar data (`CalendarEvent`, `CalendarEventImport`), and
  `CinecaSyncState` (bookkeeping for the background sync process). `api-fetcher` maintains its own
  PostgreSQL schema for batch-ingested datasets (MUR statistics, professional orders, timetable
  links).
- **What is NOT stored:** official academic records (grades, study plans, tuition fee amounts).
  These remain the responsibility of Esse3; OhMyUniversity reads them on demand (through the cache
  described below) but never becomes their system of record.

## 2. In-Memory Store (Redis) — implemented

Redis serves two distinct purposes in `api-core`; they are kept conceptually separate even though
they share the same Redis instance.

- **Esse3 response cache:** caches responses from the Esse3 client layer (career, exams, fees,
  profile data) to avoid calling Esse3's APIs on every request, reducing latency and load on Esse3
  and helping meet the < 2.0s sync target (SDD 1.2). Cached entries use a bounded, time-limited
  expiration so that cached data—some of which is sensitive—never persists in Redis longer than
  strictly necessary, in line with GDPR data minimization principles (RAD 3.3.8). PostgreSQL, not
  Redis, remains the authoritative store for this data.
- **Refresh token store:** the refresh token issued at login (separate from the short-lived JWT,
  see SDD 1.2/3.5) is held in Redis, not PostgreSQL. Logging out deletes the refresh token from
  Redis, immediately invalidating the ability to obtain a new JWT for that session.
- Redis does **not** cache Moodle data: Moodle is surfaced to students only as an external link
  (SDD 1.1), not as data retrieved or cached by the system.

## 3. Document Store (MongoDB) — planned, not yet implemented

MongoDB is planned exclusively for the chat feature (RAD FR-1.5.3), which has not been implemented
yet. `ChatMessage` data is expected to live in MongoDB once the feature is built, not in
PostgreSQL—this is a deliberate separation from the relational system of record, since chat
content does not require the same transactional guarantees as academic or calendar data. No chat
collection or schema exists today.

## 4. Flat Files and Object Storage — not planned for the current scope

No flat-file or object storage subsystem (e.g. S3-compatible storage) is currently planned. Should
a future requirement need one (e.g. storing didactic material uploads), this section will be
updated accordingly rather than assuming a specific provider in advance.