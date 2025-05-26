# Module: Intake Parser

**Endpoint**: `POST /intake`

**OperationId**: `parseData`

---

## Request: `ParseRequest`

```json
{
  "data": "string",            # Raw input (text, JSON, CSV row)
  "format": "text|json|csv"    # Data format hint
}
```

- **data**: The raw data to parse.
- **format**: Hint for the adapter to select parsing logic.

## Response: `ParseResponse`

```json
{
  "echoforms": [ Echoform ],
  "tension_map": { "<echoform_id>": <tension_score> }
}
```

- **echoforms**: List of extracted symbolic atoms.
- **tension_map**: Mapping from echoform ID to tension score.

## Core Logic

1. **Normalization**:  
   - Convert vectors to unit ℓ² norm.

2. **Tension Detection**:  
   - Use NLP (e.g., spaCy) and regex patterns to identify negations, paradox indicators, numeric thresholds.
   - Assign each echoform a tension score ∈ [0,1].

3. **Echoform Creation**:  
   - Generate `Echoform` objects:
     ```python
     Echoform(
       id=UUID,
       symbol=str,
       vector=List[float],
       structure=Dict,
       timestamp=datetime
     )
     ```