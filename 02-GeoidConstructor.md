# Module: Geoid Constructor

**Endpoint**: `POST /geoids/build`

**OperationId**: `buildGeoids`

---

## Request: `BuildRequest`

```json
{
  "echoforms": [ Echoform ]
}
```

- **echoforms**: List of `Echoform` objects to cluster.

## Response: `BuildResponse`

```json
{
  "geoids": [ Geoid ]
}
```

- **geoids**: List of clusters representing grouped echoforms.

## Core Logic

1. **Clustering**  
   - Apply threshold θ to echoform vectors to form spherical clusters (Geoids).

2. **Rotation Update**  
   - For each geoid G:
     - Compute local tension = ∑ tension(e) for e ∈ G.
     - Update rotation vector ΔR ∝ α · local_tension per axis.

3. **Geoid Representation**  
   ```python
   Geoid(
     id=UUID,
     axes=["temporal","cultural","logical"],
     rotation=List[float],
     echoform_ids=List[UUID],
     scar_ids=List[UUID]
   )
   ```