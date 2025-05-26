# Module: Vault Module

Provides persistent, versioned memory operations.

---

## Endpoints

- **Checkpoint**: `POST /vault/checkpoint`  
  - Creates a micro-checkpoint snapshot.  
  - **Response**: `SnapshotMeta`

- **Reload**: `POST /vault/reload`  
  - Body: `{ version_id: UUID }`  
  - Reloads a snapshot with per-axis decay.  
  - **Response**: `SnapshotMeta`

- **List Versions**: `GET /vault/versions`  
  - Returns array of `SnapshotMeta`.

- **Diff Snapshots**: `GET /vault/versions/{a}/diff/{b}`  
  - **Response**: `DiffReport`

- **Merge Snapshots**: `POST /vault/merge`  
  - Body: `{ a: UUID, b: UUID }`  
  - Returns merged `SnapshotMeta`.

## Data Models

- **SnapshotMeta**  
  ```json
  {
    "version_id": "string",
    "parent_id":  "string|null",
    "created":    "date-time",
    "hash":       "string"
  }
  ```

- **DiffReport**  
  ```json
  {
    "added":   ["UUID"],
    "removed": ["UUID"],
    "changed": ["UUID"]
  }
  ```

## Core Logic

1. **Event Sourcing**  
   - Store scars and RuleMutations in JSONL event log.

2. **Micro-Checkpoint**  
   - Dump current Geoid+Scar graph and sign via HMAC.

3. **Reload with Decay**  
   - Apply Axiom 2 decay to scar intensities.

4. **Merge with De-duplication**  
   - Weighted union; unify events by ID.
