Complete SOW

Statement of Work (SOW)

Architecture Design – Decentralized Civilian Tactical Operations Center (TOC)

1. Purpose

This document defines the architecture, trust model, and non‑functional requirements for a civilian‑focused Tactical Operations Center (TOC) software platform. The goal is to provide a privacy‑preserving, decentralized, zero‑trust coordination system that operates locally on user devices and enables secure collaboration between individuals, teams, and trusted external teams.

This SOW is intended to guide senior developers and architects. Ambiguity should be treated as a defect.

2. Core Architectural Principles (Non‑Negotiable)

User Data Ownership

All data is owned by the end user.

No developer‑controlled servers store or process user operational data.

Decentralized by Default

No central authority, database, or identity provider.

System must function without internet connectivity.

Zero Trust Model

No implicit trust between devices, users, leaders, or teams.

Every action must be authenticated, authorized, and cryptographically verifiable.

Open Source Transparency

100% of source code is open and auditable.

No hidden telemetry, analytics, or backchannels.

Civilian‑Legal and Ethical

The platform must avoid military‑only dependencies, classifications, or assumptions.

Features must be general‑purpose and defensible for civilian use cases.

3. Supported Platforms

The architecture must support the following platforms:

Windows (x64)

macOS (Apple Silicon + x64)

Android

iOS

Platform Strategy

A shared core logic layer (business logic, crypto, sync) is mandatory.

Platform‑specific UI layers are thin wrappers around the core.

Core must be deterministic and testable independent of UI.

4. Identity & Cryptographic Foundations

4.1 Device Identity

Each device generates its own cryptographic identity on first launch.

Identity consists of:

Public/private keypair

Device identifier (non‑PII)

Private keys never leave the device.

4.2 User Identity

A user is represented as one or more trusted devices.

No usernames, emails, or phone numbers are required.

Identity is capability‑based, not account‑based.

4.3 Trust Establishment

Trust is established explicitly via:

QR code exchange

Out‑of‑band key verification

Signed invitations

Automatic discovery is forbidden.

5. Team Model

5.1 Team Definition

A Team is a cryptographic trust group consisting of:

A unique team identifier

A set of trusted identities

A permission graph

Teams do not exist on servers; they exist as shared cryptographic state.

5.2 Leadership

Teams may have multiple leaders.

Leader is a role granted via signed capability.

Leadership permissions are scoped and revocable.

No leader has global authority outside their granted scope.

5.3 Membership

Membership is explicit and opt‑in.

Leaders approve members by issuing signed permissions.

Members receive only authorized subsets of team data.

6. Inter‑Team Federation

6.1 Trust Between Teams

Teams are isolated by default.

Teams may establish federated trust relationships with other teams.

6.2 Data Sharing Rules

Only explicitly selected objects (e.g., tasks) may be shared.

Shared data must:

Be cryptographically signed

Have explicit scope

Support expiration and revocation

No full database sharing is permitted.

7. Data Storage

7.1 Local Storage

All operational data is stored locally.

Data must be encrypted at rest using modern, audited cryptography.

7.2 Data Model

Event‑based or append‑only preferred.

Must support:

Conflict resolution

Partial replication

Auditable history

8. Synchronization & Networking

8.1 Connectivity Modes

The system must support:

Offline‑only operation

Local network sync

Internet‑based peer sync

Central relay servers are optional but must be user‑controlled.

8.2 Sync Rules

Sync is explicit and user‑approved.

No background sync without consent.

All sync traffic is encrypted end‑to‑end.

9. Revocation & Key Rotation

Members can be removed at any time.

Revocation must:

Prevent future access

Not require re‑keying the entire team unless necessary

Key rotation must be supported without data loss.

10. Threat Model (High‑Level)

Defended Against

Network eavesdropping

Compromised infrastructure

Malicious external actors

Accidental data leakage

Not Defended Against

Fully compromised user devices

Malicious insiders with valid permissions

Physical coercion

11. Out of Scope (Explicit)

Cloud‑hosted SaaS backend

Central user accounts

Advertising, analytics, or telemetry

AI/ML decision‑making systems

12. Acceptance Criteria

Architecture is considered complete when:

A new device can operate fully offline

Two devices can securely exchange team data without servers

Teams can share and revoke tasks across trust boundaries

Independent third parties can audit and verify privacy claims

13. Deliverables

Architecture diagrams

Cryptographic protocol documentation

Data model specification

Platform abstraction strategy

Security review checklist

End of SOW

