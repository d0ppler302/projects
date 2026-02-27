# Statement of Work (SOW)

## Transport Abstraction Layer (TAL) – Decentralized Civilian TOC Platform

---

## 1. Purpose

This SOW defines the **Transport Abstraction Layer (TAL)** for the decentralized civilian Tactical Operations Center (TOC) platform. TAL ensures that the **core sync engine and business logic are transport-agnostic**, enabling secure, reliable data synchronization over a variety of physical and network mediums, including constrained and offline environments.

The TAL SOW complements the Architecture SOW and Sync & Data Model SOW by providing clear interfaces and rules for transport implementation.

---

## 2. Design Goals (Non-Negotiable)

1. **Transport-Agnostic Core**

   * Core sync logic must not make assumptions about the underlying transport.

2. **Reliability Across Environments**

   * Must operate over Wi-Fi, Bluetooth, NFC, radio mesh (e.g., Meshtastic), and internet links.

3. **Graceful Degradation**

   * Sync must degrade safely in low-bandwidth or high-latency environments.

4. **Secure by Design**

   * Transport layer is **not a trust mechanism**.
   * All data must remain end-to-end encrypted, regardless of transport.

---

## 3. TAL Interface Requirements

### 3.1 Transport Modules

Each transport must implement a standard interface:

* `connect(peer_id)` — establish a communication channel
* `send(event_batch)` — send one or more events
* `receive()` — provide incoming events in order
* `disconnect()` — terminate the connection cleanly
* `get_status()` — report bandwidth, latency, reliability metrics

### 3.2 Transport Metadata

* Maximum payload size
* Latency expectations
* Reliability guarantees
* Security capabilities (e.g., encryption at transport layer)

### 3.3 Plug-and-Play

* TAL must allow multiple transports to coexist
* Core engine selects transport based on **policy, environment, and transport health**
* Transport modules are swappable without code changes to sync engine

---

## 4. Supported Transport Classes (Mandatory)

1. **Wi-Fi / LAN / Wi-Fi Direct**
2. **Bluetooth LE / Classic**
3. **NFC (Bootstrap & Handoff)**
4. **Radio / Mesh (Meshtastic / LoRa)**
5. **Internet (Optional)**

Each transport must comply with the TAL interface and security requirements.

---

## 5. Sync Semantics Over Transport

* Events are **always ordered via logical clocks**
* Transport must support **chunked and resumable batches**
* Duplicate or out-of-order delivery must be handled by sync engine, not TAL
* TAL may provide optional prioritization hints (e.g., critical events first)

---

## 6. Real-Time vs Deferred Modes

* TAL must support both:

  * **Deferred / store-and-forward**
  * **Near-real-time / streaming**
* Real-time mode is **transport- and policy-dependent**
* Core engine must not depend on real-time availability for correctness

---

## 7. Failure Handling

* Connection loss, peer unavailability, and packet loss must not corrupt core state
* Retries must be **idempotent**
* TAL must report failures for logging and user feedback

---

## 8. Security Requirements

* TAL does not replace end-to-end crypto; all events are encrypted and signed by the core
* Transport-layer encryption is optional but must not override core cryptography
* No transport module may implicitly grant trust based on physical proximity, pairing, or network reachability

---

## 9. Extensibility & Testing

* TAL must allow new transports to be added with minimal effort
* Each transport must provide **simulated testing hooks**
* Test harness must allow testing sync over constrained links and simulated failures

---

## 10. Acceptance Criteria

* Core sync engine functions identically across all implemented transports
* Event batching, ordering, and retries work as expected
* Real-time and deferred modes operate according to policy and transport capabilities
* Unauthorized or out-of-scope events are never propagated
* Transport modules can be swapped without changing sync engine code

---

## 11. Deliverables

* TAL interface specification
* Transport module templates for Wi-Fi, BLE, NFC, and radio mesh
* Test harness for transport simulation and validation
* Documentation for adding new transport modules

---

**End of SOW**
