# Bitmap Activity Standard (BAS)

> This repository defines the canonical Bitmap Activity Standard (BAS) v0.1 specification.
> Any implementation claiming BAS v0.1 compatibility MUST conform to the JSON schema
> defined in the Specification section of this document.

Version: v0.1

Status: Stable

## Overview

The Bitmap Activity Standard (BAS) defines a neutral, open, and reproducible method for recording on-chain territory activity within the Bitmap ecosystem using Ordinals native parent/child provenance.

This standard does not require protocol changes.
It leverages existing Bitcoin and Ordinals primitives.

The goal is simple:
Enable verifiable, machine-readable, and interoperable territory activity logs for Bitmap districts.

## Why BAS v0.1?

Current Bitmaps are static assets. BAS v0.1 introduces a standard way to log
on-chain activity, turning blocks into verifiable, hierarchical databases.

### Core Architecture

- **Parent:** The Bitmap Inscription ID (The Root / Territory)
- **Child:** A JSON inscription linked to the Parent via native Ordinals parent-child provenance (The Event / Log)
- **Sequence:** Deterministic integer ordering
- **Version:** Explicit schema versioning for forward compatibility

---

## Parent-Child Requirement

A BAS child inscription MUST be created as a **native Ordinals child** of the parent inscription.

Writing `parent_inscription_id` inside the JSON content is **not sufficient** to establish provenance.

Native Ordinals parent-child provenance requires two conditions met simultaneously during inscription creation:

1. Including the `parent` tag in the inscription envelope.
2. Spending the parent inscription as an input of the transaction that creates the child.

This relationship cannot be added retroactively.
Only inscriptions that meet both conditions are recognized as children by Ordinals indexers.

---

## Specification

This section defines the formal JSON structure required for BAS v0.1 compliance.

### Record Types

BAS v0.1 defines two record types with different field requirements:

- `"territory_event"` — Records an activity or event in the territory
- `"territory_standard"` — Records a rule declaration or standard definition for the territory

### Required Fields — territory_event

- `type` — Must equal `"territory_event"`
- `version` — Must equal `"bitmap_activity_v1"`
- `territory` — The full bitmap name (e.g., `"3666.bitmap"`)
- `parent_inscription_id` — The inscription ID of the native parent bitmap inscription
- `sequence` — Deterministic incremental integer (starting at 1)
- `timestamp` — UTC timestamp in ISO 8601 format (e.g., `"2026-03-05T00:00:00Z"`)

### Required Fields — territory_standard

- `type` — Must equal `"territory_standard"`
- `version` — Must equal `"bitmap_activity_v1"`
- `territory` — The full bitmap name
- `parent_inscription_id` — The inscription ID of the native parent bitmap inscription
- `sequence` — Deterministic incremental integer

> `timestamp` is optional for `territory_standard`. The live Sequence 3 on-chain does not include it.

### Optional Fields (both types)

- `timestamp` (string, ISO 8601) — Required for `territory_event`, optional for `territory_standard`
- `event` (string)
- `description` (string)
- `activity` (object)
- `standard` (object)
- `metadata` (object)

### Version Field

The `version` field MUST equal `"bitmap_activity_v1"` for all BAS v0.1 inscriptions.
This value is the fixed on-chain identifier and MUST NOT change for BAS v0.1 compatibility.

> Note: The on-chain Sequence 3 inscription references "Bitmap Activity Layer" in its description field — this was the earlier working name. The official specification name is **Bitmap Activity Standard (BAS)**. The `version` value `"bitmap_activity_v1"` is preserved as-is for backward compatibility.

### Canonical Schema Examples

**territory_event:**

```json
{
  "type": "territory_event",
  "version": "bitmap_activity_v1",
  "territory": "3666.bitmap",
  "parent_inscription_id": "809660b04d441fec9ac760c4bc5484f94a78d17f3cccb8fe0ad42310877d4df7i0",
  "sequence": 1,
  "timestamp": "2026-03-05T00:00:00Z",
  "event": "genesis_activation",
  "activity": {
    "description": "First BAS activity on territory 3666.bitmap"
  }
}
```

**territory_standard:**

```json
{
  "type": "territory_standard",
  "version": "bitmap_activity_v1",
  "territory": "3666.bitmap",
  "parent_inscription_id": "809660b04d441fec9ac760c4bc5484f94a78d17f3cccb8fe0ad42310877d4df7i0",
  "sequence": 3,
  "standard": {
    "name": "Bitmap Activity Standard",
    "rules": ["..."]
  },
  "description": "Formal declaration of Bitmap Activity Standard v0.1 for structured territorial activity."
}
```

### Deterministic Ordering

Ordering of events MUST be determined exclusively by the `sequence` field.
Block height or inscription number MUST NOT define logical order.

### Forward Compatibility

Future specification versions will increment the version label (BAS v0.2, BAS v1.0, etc.).
The on-chain `version` field value `"bitmap_activity_v1"` is fixed for all BAS v0.1 implementations.

---

## Why This Standard Exists

Bitmap represents Bitcoin blocks as sovereign digital territories.

However, there is currently no unified structure for:

- Recording first activity within a territory
- Declaring territory genesis actions
- Logging structured events
- Enabling AI agents and sovereign entities (EONs) to interpret territory state
- Creating interoperable on-chain registries

BAS provides a minimal and extensible structure to solve this.

---

## Core Principles

1. Bitcoin-native
2. Ordinals-native
3. Parent/Child provenance-based (native Ordinals envelope, not JSON reference)
4. Machine-readable (JSON)
5. Neutral and permissionless
6. Backwards compatible
7. Extensible

---

## Activity Types

- `genesis` — First recorded activity on a territory
- `first_activity` — Initial notable action
- `declaration` — Formal territory declaration
- `agent_entry` — Agent or sovereign entity (EON) crossing into the territory
- `custom` — Extensible for future use

---

## How to Implement BAS v0.1

### 1. Identify Parent

Locate your Bitmap inscription ID. This acts as the root (territory anchor).

Example:
```
809660b04d441fec9ac760c4bc5484f94a78d17f3cccb8fe0ad42310877d4df7i0
```

### 2. Format Activity JSON

Create a JSON object following the BAS v0.1 specification for the appropriate record type.

Minimum required structure for `territory_event`:

```json
{
  "type": "territory_event",
  "version": "bitmap_activity_v1",
  "territory": "YOUR_BITMAP.bitmap",
  "parent_inscription_id": "PARENT_ID",
  "sequence": 1,
  "timestamp": "YYYY-MM-DDTHH:MM:SSZ"
}
```

### 3. Inscribe as Native Child

Use your preferred Ordinals tool (UniSat, Ordinals Wallet, CLI tools) and **explicitly set the parent inscription** when creating the child. The `sequence` value MUST increment deterministically.

---

## Live Implementation Example

The Bitmap Activity Standard (BAS) is already live.

The following territory implements BAS v0.1 using real on-chain inscriptions:

- Territory: **3666.bitmap**
- Parent inscription: `809660b04d441fec9ac760c4bc5484f94a78d17f3cccb8fe0ad42310877d4df7i0`
- Anchor address: `bc1pv40yl4jyne02pulahpdrl98d2x3z9yd7g3c9kzm4ktyz69mdwe2qhmmrqz`

Full implementation reference: [examples/3666-live-implementation.json](examples/3666-live-implementation.json)

### On-Chain Sequence (3666.bitmap)

**Parent (Bitmap Root)**
`809660b04d441fec9ac760c4bc5484f94a78d17f3cccb8fe0ad42310877d4df7i0`

---

**Sequence 1 — Genesis Activation**
`3dad893ee2df34e3c26db1cffad4c03a2929668f2ffe7ec8d25aade1a18e2125i0`
Block: `939430` — 2026-03-05

---

**Sequence 2 — Lightning Proof (on-chain record)**
`3eac40940bf02eeac41b0f118950d15ebe8b33f04f79413d72e7003799a33e88i0`
Block: `939435` — 2026-03-05

> This inscription records on-chain a Lightning payment that occurred off-chain. The payment (21 sats, payment hash `2837ecabef7d3915297fc610dc19308743fd891f701fc1c8b2cc0a1c26ecb10e`) took place via the Lightning Network on 2026-02-18. The BAS inscription anchors that off-chain event to Bitcoin finality through the parent-child provenance chain.

---

**Sequence 3 — Territory Standard Declaration**
`fd0b09c88ad2b25fd2b33b36c2c12ab01196b998f059ac7f79495cd37a88abdci0`
Block: `939438` — 2026-03-05

> Type: `territory_standard`. No `timestamp` field (valid per BAS v0.1 spec for this type).

---

**Sequence 4 — Agent Entry (Proteon / Myceleon Network)**
`75796d260ab6e669fa352eb7c0a159ba6fcddc2477a7893acd19be8db496fdc4i0`
Block: `946493` — 2026-04-24

> Actor: **Proteon** — Gate: `xrswapgate.bitmap` — Network: `myceleon`

---

## Reference Territories

The following territories are known live implementations of BAS v0.1:

1. **3666.bitmap**
   - Parent inscription: `809660b04d441fec9ac760c4bc5484f94a78d17f3cccb8fe0ad42310877d4df7i0`
   - Implementation file: [examples/3666-live-implementation.json](examples/3666-live-implementation.json)

Additional territories can be added via pull request once they demonstrate deterministic native parent-child activity logging compliant with BAS v0.1.

---

## Governance

BAS evolves through simple versioned releases.

- BAS v0.1 — Initial stable release (current)
- Future updates will increment the specification version (v0.2, v1.0, etc.)
- Backward compatibility will be preserved whenever possible.
- The on-chain `version` value `"bitmap_activity_v1"` is fixed for all BAS v0.1 implementations.

Proposals for improvements can be submitted via GitHub Issues.
