# Module: Reactor Gate

**Endpoint**: `POST /reactor`

**OperationId**: `runReactor`

---

## Request: `ReactorRequest`

```json
{
  "echoforms": [ Echoform ],
  "iterations": integer,               # ≥1
  "explain_level": "summary|full"      # default: full
}
```

- **iterations**: Number of inference cycles.
- **explain_level**: Controls output detail.

## Response: `ReactorResponse`

```json
{
  "geoids": [ Geoid ],
  "scars":  [ Scar ]
}
```

- **geoids**: Updated clusters.
- **scars**: New inference events.

## Core Logic

1. **Deduction**  
   - Apply rules via Modus Ponens.

2. **Abduction**  
   - Generate hypotheses for unexplained echoforms based on vector similarity ≥ θ_abd.

3. **Contradiction-Driven**  
   - Detect pairs (φ, ¬φ) and emit `Scar(type="contradiction")`, then spawn `resolve(φ,¬φ)`.

4. **Event Logging**  
   - Append each inference as `Scar` to in-memory event log.

5. **Micro-Checkpoint Trigger**  
   - After N scars or T seconds, snapshot via `/vault/checkpoint`.