# Module: Output Translator

Generates human-consumable representations.

---

## Endpoints

- **Translate**: `POST /translate`  
  - Inputs: `ReactorResponse` or `DiffReport`  
  - Output: HTML snippets or JSON summary.

- **Get Geoid**: `GET /geoids/{id}?level=summary|full`  
  - Returns tiered explainability for one Geoid.

## Data Models

- **TranslateRequest**  
  ```json
  {
    "geoids":      [ Geoid ],
    "scars":       [ Scar ],
    "html_diff":   "string (if any)",
    "explain_level": "summary|full"
  }
  ```

- **TranslateResponse**  
  ```json
  {
    "html":       "string",
    "summary":    { /* key fields */ },
    "details":    { /* full trail if requested */ }
  }
  ```

## Core Logic

1. **Summary View**  
   - Top-3 echoforms, key scar, resonance score.

2. **Full View**  
   - All scars, vectors, HTML diff table.

3. **HTML Diff Rendering**  
   - Use pre-defined Mustache-like template (see Appendix D).