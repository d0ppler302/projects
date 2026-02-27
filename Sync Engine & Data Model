# Statement of Work (SOW)

## Sync Engine & Data Model – Decentralized Civilian TOC Platform

---

## 1. Purpose

This document defines the **data model, synchronization strategy, and conflict-resolution rules** for the decentralized civilian Tactical Operations Center (TOC) platform.

The intent is to eliminate ambiguity so that multiple developers (human or AI) implement **compatible, predictable, and secure synchronization behavior**, even under offline, low-bandwidth, and adversarial network conditions.

This SOW builds directly on the approved Architecture SOW.

---

## 2. Design Goals (Non-Negotiable)

1. **Offline-First**

   * All features must function without connectivity.

2. **Transport-Agnostic Sync**

   * Sync logic must operate identically over Wi-Fi, Bluetooth, radio mesh, NFC handoff, or internet.

3. **Deterministic Outcomes**

   * Given the same inputs, all honest nodes converge on the same state.

4. **Partial Replication**

   * Devices only store data they are authorized to access.

5. **Auditability**

   * Changes must be inspectable and attributable.

---

## 3. Data Model Overview

### 3.1 Event-Sourced Core (Required)

The system shall use an **append-only, event-sourced data model**.

* No mutable global state
* No last-write-wins shortcuts
* State is derived by replaying ordered events

This model is mandatory to support:

* Auditing
* Revocation semantics
* Constrained transport replay

---

## 4. Event Structure

Each event must include:

* Event ID (cryptographically unique)
* Author identity (public key reference)
* Target object ID (task, team, membership, etc.)
* Event type
* Payload
* Logical timestamp
* Cryptographic signature

Events are immutable once created.

---

## 5. Object Types (MVP Scope)

The following objects must be represented purely via events:

* Team
* Member
* Role / Capability
* Task
* Task assignment
* Task status update
* Inter-team task share

Maps, attachments, and live telemetry are explicitly out of scope for MVP sync.

---

## 6. Event Ordering & Causality

### 6.1 Logical Clocks

* Events must use logical clocks (e.g., Lamport or hybrid logical clocks).
* Wall-clock time may be included for UX but **must not** drive conflict resolution.

---

## 7. Conflict Resolution Rules

### 7.1 Authority-Based Resolution

Conflicts are resolved based on **capability authority**, not time.

Examples:

* Only identities with `TaskEdit` capability may modify a task.
* Only identities with `LeaderGrant` capability may add leaders.

Unauthorized events are ignored but retained for audit.

---

### 7.2 Concurrent Valid Events

If two valid, authorized events conflict:

* Both events are preserved
* Resulting object enters a **conflicted state**
* Manual resolution is required

Silent overwrites are forbidden.

---

## 8. Sync Protocol

### 8.1 Sync Unit

* Sync operates on **event batches**, not object snapshots.
* Batches must be chunkable and resumable.

---

### 8.2 Sync Handshake

During sync, peers must exchange:

* Known event ranges
* Authorization scopes
* Transport constraints

Peers must not request or accept unauthorized events.

---

## 9. Partial Replication Rules

* Devices only sync events they are authorized to read.
* Authorization is evaluated per event.
* Loss of authorization prevents future sync but does not erase local history unless explicitly requested.

---

## 10. Revocation Semantics

### 10.1 Member Revocation

* Revocation prevents receipt of new events.
* Past events remain readable unless data is explicitly re-keyed.

---

### 10.2 Leader Compromise

* Compromised leaders can be removed by other leaders with sufficient authority.
* New events signed by revoked leaders are rejected.

Automatic team recreation is forbidden.

---

## 11. Garbage Collection & Compaction

* Event logs may be compacted locally.
* Compaction must not:

  * Change derived state
  * Remove audit-critical events

---

## 12. Failure Modes

The system must handle:

* Partial sync
* Duplicate events
* Out-of-order delivery
* Malicious but authorized event spam

No failure may corrupt existing state.

---

## 13. Out of Scope (Explicit)

* Real-time collaborative editing
* Automatic conflict resolution heuristics
* Centralized indexing or search

---

## 14. Acceptance Criteria

This SOW is satisfied when:

* Two offline devices independently modify data and later converge safely
* Conflicts are surfaced, not hidden
* Revoked members cannot receive new data
* Sync works over constrained transports without data loss

---

## 15. Deliverables

* Event schema specification
* Sync handshake definition
* Conflict state UX guidelines
* Reference test vectors

---

**End of SOW**
