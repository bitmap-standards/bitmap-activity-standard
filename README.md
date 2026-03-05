# Bitmap Activity Standard (BAS)

Version: v0.1 (Draft)

## Overview

The Bitmap Activity Standard (BAS) defines a neutral, open, and reproducible method for recording on-chain territory activity within the Bitmap ecosystem using Ordinals provenance (parent/child structure).

This standard does not require protocol changes.
It leverages existing Bitcoin and Ordinals primitives.

The goal is simple:
Enable verifiable, machine-readable, and interoperable territory activity logs for Bitmap districts.

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
