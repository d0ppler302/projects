Complete SOW

# Statement of Work (SOW)

## Architecture Design – Decentralized Civilian Tactical Operations Center (TOC)

---

## 1. Purpose

This document defines the **architecture, trust model, and non‑functional requirements** for a civilian‑focused Tactical Operations Center (TOC) software platform. The goal is to provide a **privacy‑preserving, decentralized, zero‑trust coordination system** that operates locally on user devices and enables secure collaboration between individuals, teams, and trusted external teams.

This SOW is intended to guide senior developers and architects. Ambiguity should be treated as a defect.

---

## 2. Core Architectural Principles (Non‑Negotiable)

1. **User Data Ownership**

   * All data is owned by the end user.
   * No developer‑controlled servers store or process user operational data.

2. **Decentralized by Default**

   * No central authority, database, or identity provider.
   * System must function without internet connectivity.

3. **Zero Trust Model**

   * No implicit trust between devices, users, leaders, or teams.
   * Every action must be authenticated, authorized, and cryptographically verifiable.

4. **Open Source Transparency**

   * 100% of source code is open and auditable.
   * No hidden telemetry, analytics, or backchannels.

5. **Civilian‑Legal and Ethical**

   * The platform must avoid military‑only dependencies, classifications, or assumptions.
   * Features must be general‑purpose and defensible for civilian use cases.

---

## 3. Supported Platforms

The architecture must support the following platforms:

* Windows (x64)
* macOS (Apple Silicon + x64)
* Android
* iOS

### Platform Strategy

* A **shared core logic layer** (business logic, crypto, sync) is mandatory.
* Platform‑specific UI layers are thin wrappers around the core.
* Core must be deterministic and testable independent of UI.

---

## 4. Identity & Cryptographic Foundations

### 4.1 Device Identity

* Each device generates its own **cryptographic identity** on first launch.
* Identity consists of:

  * Public/private keypair
  * Device identifier (non‑PII)
* Private keys never leave the device.

### 4.2 User Identity

* A user is represented as one or more trusted devices.
* No usernames, emails, or phone numbers are required.
* Identity is **capability‑based**, not account‑based.

### 4.3 Trust Establishment

* Trust is established explicitly via:

  * QR code exchange
  * Out‑of‑band key verification
  * Signed invitations

Automatic discovery is forbidden.

---

## 5. Team Model

### 5.1 Team Definition

A **Team** is a cryptographic trust group consisting of:

* A unique team identifier
* A set of trusted identities
* A permission graph

Teams do not exist on servers; they exist as **shared cryptographic state**.

### 5.2 Leadership

* Teams may have **multiple leaders**.
* Leader is a role granted via signed capability.
* Leadership permissions are scoped and revocable.

No leader has global authority outside their granted scope.

### 5.3 Membership

* Membership is explicit and opt‑in.
* Leaders approve members by issuing signed permissions.
* Members receive **only authorized subsets** of team data.

---

## 6. Inter‑Team Federation

### 6.1 Trust Between Teams

* Teams are isolated by default.
* Teams may establish **federated trust relationships** with other teams.

### 6.2 Data Sharing Rules

* Only explicitly selected objects (e.g., tasks) may be shared.
* Shared data must:

  * Be cryptographically signed
  * Have explicit scope
  * Support expiration and revocation

No full database sharing is permitted.

---

## 7. Data Storage

### 7.1 Local Storage

* All operational data is stored locally.
* Data must be encrypted at rest using modern, audited cryptography.

### 7.2 Data Model

* Event‑based or append‑only preferred.
* Must support:

  * Conflict resolution
  * Partial replication
  * Auditable history

---

## 8. Synchronization & Networking

### 8.1 Connectivity Philosophy

The system **must not assume the presence of the public internet**. Synchronization and data exchange must be transport-agnostic and operate over any available bearer capable of moving bytes.

Networking is treated as a **pluggable transport layer**, not a core dependency.

---

### 8.2 Supported Transport Classes (Required)

The architecture must support the following **non-internet and constrained transports** as first-class citizens:

1. **Direct Wi-Fi**

   * Wi‑Fi Direct
   * Local AP / ad-hoc modes
   * Peer discovery without centralized services

2. **Bluetooth**

   * Bluetooth LE (GATT-based sync)
   * Bluetooth Classic where available

3. **NFC (Bootstrap + Transfer)**

   * Used for:

     * Identity exchange
     * Trust establishment
     * Initial capability transfer
   * NFC is not required to handle bulk data but must support handoff to another transport

4. **Radio / Mesh Interfaces**

   * Meshtastic-compatible LoRa devices
   * External radios connected via USB / serial / BLE
   * Extremely low-bandwidth, high-latency links must be supported

5. **Removable / Offline Transfer (Optional but Recommended)**

   * QR code payloads
   * File-based export/import for air-gapped environments

---

### 8.3 Transport Abstraction Layer

Developers must implement a **Transport Abstraction Layer (TAL)** with the following properties:

* Core sync logic is completely unaware of the underlying transport
* Transports expose:

  * Max payload size
  * Reliability guarantees
  * Latency expectations
* Sync engine adapts behavior accordingly

No transport-specific logic is permitted in business logic.

---

### 8.4 Sync Behavior Over Constrained Links

When operating over constrained transports (Bluetooth, radio, mesh):

* Data must be chunked and resumable
* Priority must be given to:

  * Tasks
  * Position updates (if enabled)
  * Critical messages
* Large objects (maps, attachments) must be deferred or manually approved

---

### 8.5 Security Requirements (All Transports)

Regardless of transport:

* End-to-end encryption is mandatory
* Mutual authentication is mandatory
* No trust is derived from physical proximity alone
* Replay protection must be implemented

Transport-level security (e.g., BLE pairing) **does not replace** application-layer cryptography.

---

### 8.6 Internet Connectivity (Optional)

Internet-based sync is:

* Optional
* User-controlled
* Never required for core functionality

If used, it must obey the same zero-trust, end-to-end encrypted model.

---

## 9. Revocation & Key Rotation

* Members can be removed at any time.
* Revocation must:

  * Prevent future access
  * Not require re‑keying the entire team unless necessary

Key rotation must be supported without data loss.

---

## 10. Threat Model (High‑Level)

### Defended Against

* Network eavesdropping
* Compromised infrastructure
* Malicious external actors
* Accidental data leakage

### Not Defended Against

* Fully compromised user devices
* Malicious insiders with valid permissions
* Physical coercion

---

## 11. Out of Scope (Explicit)

* Cloud‑hosted SaaS backend
* Central user accounts
* Advertising, analytics, or telemetry
* AI/ML decision‑making systems

---

## 12. Acceptance Criteria

Architecture is considered complete when:

* A new device can operate fully offline
* Two devices can securely exchange team data without servers
* Teams can share and revoke tasks across trust boundaries
* Independent third parties can audit and verify privacy claims

---

## 13. Deliverables

* Architecture diagrams
* Cryptographic protocol documentation
* Data model specification
* Platform abstraction strategy
* Security review checklist

---

**End of SOW**
