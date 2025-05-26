# Module: Consensus Engine

Handles fusion of conflicting scars into consensus insights.

---

## Endpoints

- **Add Consensus**: `POST /consensus`  
  - Body: `ConsensusRequest`  
  - **Response**: `ConsensusEvent`

- **Bulk Consensus**: `POST /consensus/bulk`  
  - Body: `{ scar_ids: [UUID], axis: string, batch_size: int, agent_id: UUID }`

- **History**: `GET /consensus/history?since&axis`  
  - **Response**: `{ events: [ConsensusEvent] }`

## Data Models

- **ConsensusRequest**  
  ```json
  {
    "scar_ids":       ["UUID"],
    "axis":           "temporal|cultural|logical",
    "agent_id":       "UUID",
    "rationale":      "string (optional)"
  }
  ```

- **ConsensusEvent**  
  ```json
  {
    "event_id":       "UUID",
    "scar_ids":       ["UUID"],
    "new_scar_id":    "UUID",
    "axis":           "string",
    "intensity_map":  { "UUID": number },
    "threshold":      number,
    "cooldown":       number,
    "diversity_factor": number,
    "agent_id":       "UUID",
    "rationale":      "string",
    "timestamp":      "date-time"
  }
  ```

## Core Logic

1. **Vector Fusion**  
   - Weighted-average of scar vectors, normalized.

2. **Intensity Cooling**  
   - Original scars multiplied by (1 − cooldown).

3. **Symbol Registry**  
   - Ensure unique `resolve(...)` naming.

4. **Transactional Guard**  
   - All updates within a single DB transaction.

5. **Governance Metadata**  
   - Record `agent_id` and `rationale` in event.