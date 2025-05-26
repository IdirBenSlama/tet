# Kimera-SWM v0.1

**Kimera-SWM** (Symbolic-Semantic Workspace Memory) is a living-memory, contradiction-driven AI engine 
that ingests raw data (text, JSON, CSV, DB rows), discovers symbolic atoms (Echoforms), clusters them into 
rotating Geoids, runs zetetic loops (deduction, abduction, contradiction), logs every step as Scars, and 
snapshots state to a cryptographically-signed Vault. A modular Consensus Engine fuses conflicting scars 
into new insights.

## Project Structure

```
kimera-swm/
├── openapi.yaml                  # OpenAPI contract definition
├── kimeraswm.proto               # gRPC protobuf definitions
├── schemas/                      # JSON Schemas for data models
├── docs/                         # Module documentation markdowns
├── src/                          # Generated server stubs & implementation
│   └── server/                   # FastAPI or other framework code
├── tests/                        # Unit & integration tests
├── Dockerfile                    # Docker container specification
├── docker-compose.yml            # Local development with services
├── README.md                     # This file
├── .gitignore                    # Files to ignore in Git
├── requirements.txt              # Python dependencies
└── .env.example                  # Example environment variables
```

## Getting Started

### Prerequisites

- Python 3.10+
- Docker & Docker Compose (for containerized setup)
- `npm` (for OpenAPI codegen, optional)

### Local Development

1. **Clone the repo**
   ```bash
   git clone <repo_url>
   cd kimera-swm
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv .venv
   source .venv/bin/activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Set environment variables**  
   Copy `.env.example` to `.env` and update values:
   ```
   cp .env.example .env
   ```

5. **Run the API server**  
   ```bash
   uvicorn src.server.app.main:app --reload
   ```

### Containerized Setup

```bash
docker-compose up --build
```

The API will be available at `http://localhost:8000`.

## Running Tests

```bash
pytest
```

Contract tests:
```bash
schemathesis run openapi.yaml --checks all --base-url http://localhost:8000
```

## Next Steps

- Implement the handlers in `src/server` according to `openapi.yaml`.
- Develop business logic modules under `src/server/app/services`.
- Configure CI/CD to validate specs, generate code, and run tests.

