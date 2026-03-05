# Bitmap Activity Standard (BAS)

Version: v0.1 (Draft)

## Overview

The Bitmap Activity Standard (BAS) defines a neutral, open, and reproducible method for recording on-chain territory activity within the Bitmap ecosystem using Ordinals provenance (parent/child structure).

This standard does not require protocol changes.
It leverages existing Bitcoin and Ordinals primitives.

The goal is simple:
Enable verifiable, machine-readable, and interoperable territory activity logs for Bitmap districts.

## Why BAS v1?

Current Bitmaps are static assets. BAS v1 introduces a standard way to log 
on-chain activity, turning blocks into verifiable, hierarchical databases.

### Core Architecture

- **Parent:** The Bitmap Inscription ID (The Root / Territory)
- **Child:** A JSON inscription linked to the Parent (The Event / Log)
- **Sequence:** Deterministic integer ordering
- **Version:** Explicit schema versioning for forward compatibility

  ---

## Specification

This section defines the formal JSON structure required for BAS v0.1 compliance.

### Required Fields (Child Inscription)

A valid BAS child inscription MUST contain the following fields:

- `type` (string)  
  Must equal `"territory_event"` or `"territory_standard"`

- `version` (string)  
  Must equal `"bitmap_activity_v1"`

- `territory` (string)  
  The full bitmap name (e.g., `"3666.bitmap"`)

- `parent_inscription_id` (string)  
  The inscription ID of the parent bitmap

- `sequence` (integer)  
  Deterministic incremental integer (starting at 1)

- `timestamp` (ISO 8601 string)  
  UTC timestamp of event creation

### Optional Fields

- `description` (string)
- `activity` (object)
- `standard` (object)
- `metadata` (object)

### Deterministic Ordering

Ordering of events MUST be determined exclusively by the `sequence` field.  
Block height or inscription number MUST NOT define logical order.

### Forward Compatibility

Future versions of BAS MUST increment the `version` field and preserve backward readability.
  
---

## Why This Standard Exists

Bitmap represents Bitcoin blocks as sovereign digital territories.

However, there is currently no unified structure for:

- Recording first activity within a territory
- Declaring territory genesis actions
- Logging structured events
- Enabling AI agents to interpret territory state
- Creating interoperable on-chain registries

BAS provides a minimal and extensible structure to solve this.

---

## Core Principles

1. Bitcoin-native
2. Ordinals-native
3. Parent/Child provenance-based
4. Machine-readable (JSON)
5. Neutral and permissionless
6. Backwards compatible
7. Extensible

---

## Basic Structure

Each territory may define:

### 1. Parent Inscription (Territory Anchor)

Represents the territory identity (example: 3666.bitmap or equivalent block reference).

### 2. Child Activity Inscriptions

Each child inscription must:

- Reference the parent inscription
- Use structured JSON
- Include version field
- Define activity type

---

## Minimal JSON Schema (v0.1)

Example:

```json
{
  "bas_version": "0.1",
  "territory": "3666.bitmap",
  "activity_type": "genesis",
  "timestamp": 1700000000,
  "actor": "bc1p...",
  "metadata": {
    "note": "First recorded activity"
  }
}

————

Activity Types (Initial)
	•	genesis
	•	first_activity
	•	declaration
	•	agent_entry
	•	custom (extensible)

⸻

Intended Use Cases
	•	Bitmap territory logging
	•	Agent-readable district states
	•	On-chain registries
	•	Decentralized land coordination
	•	Cross-agent interoperability
	•	NAT / TRAC / future ecosystem integrations

⸻

Governance

This repository defines a neutral open draft.

Future revisions should:
	•	Remain minimal
	•	Preserve backwards compatibility
	•	Be openly discussed via issues and pull requests

---

## Live Implementation Example

The Bitmap Activity Standard (BAS) is already live.

The following territory implements BAS v0.1 using real on-chain inscriptions:

- Territory: **3666.bitmap**
- Parent inscription: `809660b04d441fec9ac760c4bc5484f94a78d17f3cccb8fe0ad42310877d4df7i0`
- Anchor address: `bc1pv40yl4jyne02pulahpdrl98d2x3z9yd7g3c9kzm4ktyz69mdwe2qhmmrqz`

Full implementation reference:

[examples/3666-live-implementation.json](examples/3666-live-implementation.json)

This file documents a deterministic sequence of child inscriptions
anchored to a single parent inscription ID.

### On-Chain Reference (3666.bitmap)

The following inscriptions form the first live BAS sequence:

- **Parent (Bitmap Root)**
  - `809660b04d441fec9ac760c4bc5484f94a78d17f3cccb8fe0ad42310877d4df7i0`

- **Sequence 1 — Genesis Activation**
  - `3dad893ee2df34e3c26db1cffad4c03a2929668f2ffe7ec8d25aade1a18e2125i0`
  - Block Height: `939430`

- **Sequence 2 — First Activity (Lightning Proof)**
  - `3eac40940bf02eeac41b0f118950d15ebe8b33f04f79413d72e7003799a33e88i0`
  - Block Height: `939435`

- **Sequence 3 — Territory Standard Declaration**
  - `fd0b09c88ad2b25fd2b33b36c2c12ab01196b998f059ac7f79495cd37a88abdci0`
  - Block Height: `939438`

---

## Reference Territories

The following territories are known live implementations of BAS:

1. **3666.bitmap**
   - Parent inscription: `809660b04d441fec9ac760c4bc5484f94a78d17f3cccb8fe0ad42310877d4df7i0`
   - Implementation file:
     [examples/3666-live-implementation.json](examples/3666-live-implementation.json)

Additional territories can be added via pull request once they
demonstrate deterministic parent/child activity logging compliant with BAS.

---

## How to Implement BAS v0.1

Follow these steps to implement the Bitmap Activity Standard.

### 1️⃣ Identify Parent

Locate your Bitmap inscription ID.  
This acts as the root (territory anchor).

Example:

`809660b04d441fec9ac760c4bc5484f94a78d17f3cccb8fe0ad42310877d4df7i0`

---

### 2️⃣ Format Activity JSON

Create a JSON object that follows the BAS v0.1 specification.

Minimum required structure:

```json
{
  "type": "territory_event",
  "version": "bitmap_activity_v1",
  "territory": "YOUR_BITMAP.bitmap",
  "parent_inscription_id": "PARENT_ID",
  "sequence": 1,
  "timestamp": "YYYY-MM-DDTHH:MM:SSZ"
}

---

### 3️⃣ Inscribe as Child

Use your preferred Ordinals tool:

- UniSat  
- Ordinals Wallet  
- CLI tools  

Ensure the inscription is explicitly linked as a child of the parent inscription ID.

The `sequence` value MUST increment deterministically.
