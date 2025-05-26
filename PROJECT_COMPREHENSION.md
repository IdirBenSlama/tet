# Project Comprehension: Kimera-SWM

## 1. Project Purpose

Kimera-SWM (Symbolic-Semantic Workspace Memory) is a living-memory, contradiction-driven AI engine. It is designed to ingest raw data from various formats (text, JSON, CSV, DB rows), discover symbolic atoms called Echoforms, and cluster these Echoforms into rotating structures called Geoids. The system then runs zetetic loops (deduction, abduction, contradiction) on this data, logging every step as Scars. It snapshots its state to a cryptographically-signed Vault, ensuring an auditable and mergeable memory. A modular Consensus Engine is responsible for fusing conflicting Scars to generate new insights.

(Source: README.md, Kimera-SWM_v0.1_System_Spec.md)

## 2. High-Level Architecture

The Kimera-SWM system follows a pipeline architecture:

**Intake -> Cluster -> Inference -> Vault**

1.  **Intake Parser**: Receives raw data and transforms it into Echoforms.
2.  **Geoid Constructor**: Clusters the incoming Echoforms into Geoids.
3.  **Reactor Gate**: Performs inference operations (deduction, abduction, contradiction) on the Geoids, generating Scars.
4.  **Vault Module**: Manages the storage and retrieval of system snapshots, allowing for versioning, diffing, and merging of states.

The system also includes a **Consensus Engine** that processes Scars to resolve conflicts and derive new knowledge, and an **Output Translator** to present the system's state and findings in human-readable formats. Client applications can interact with the system via REST or gRPC APIs.

```
 [ Client ]
     │  REST / gRPC
     ▼
╔════════════╗       ╔════════════╗       ╔════════════╗
║ Intake     ║──────▶║ Geoid      ║──────▶║ Reactor    ║
║ Parser     ║       ║ Constructor║       ║ Gate       ║
╚════════════╝       ╚════════════╝       ╚═════╦══════╝
                                                │
                                                ▼
                                           ╔════════════╗
                                           ║ Vault      ║
                                           ║ Module     ║
                                           ╚═════╦══════╝
                                                 │
      ┌──────────────────────────────────────────┴───────────┐
      │    Consensus Engine     │     Output Translator     │
      └─────────────────────────────────────────────────────┘
```

(Source: Kimera-SWM_v0.1_System_Spec.md)

## 3. Core Components/Modules

*   **Intake Parser**:
    *   **Function**: Ingests raw data (text, JSON, CSV) and parses it into fundamental symbolic units called Echoforms. It also identifies negations and patterns to create a "tension map".
    *   **API Endpoint**: `POST /intake`
    *   (Source: Kimera-SWM_v0.1_System_Spec.md, openapi.yaml)

*   **Geoid Constructor**:
    *   **Function**: Takes Echoforms as input and clusters them into Geoids based on a θ-threshold. It updates the rotation of these Geoids according to tension scores.
    *   **API Endpoint**: `POST /geoids/build`
    *   (Source: Kimera-SWM_v0.1_System_Spec.md, openapi.yaml)

*   **Reactor Gate**:
    *   **Function**: Executes inference cycles on Echoforms within Geoids. This involves processes of deduction, abduction, and contradiction detection. It logs the outcomes of these processes as Scars and can trigger rule mutations. It also manages micro-checkpoints of the system state.
    *   **API Endpoint**: `POST /reactor`
    *   (Source: Kimera-SWM_v0.1_System_Spec.md, openapi.yaml)

*   **Vault Module**:
    *   **Function**: Manages persistent storage of the system's memory snapshots. It supports operations like creating checkpoints, reloading previous states, listing available versions, diffing between two versions, and merging different snapshots.
    *   **API Endpoints**: `POST /vault/checkpoint`, `POST /vault/reload`, `GET /vault/versions`, `GET /vault/versions/{a}/diff/{b}`, `POST /vault/merge`
    *   (Source: Kimera-SWM_v0.1_System_Spec.md, openapi.yaml)

*   **Consensus Engine**:
    *   **Function**: Fuses conflicting Scars to generate new insights or resolve contradictions. It uses a weighted-average fusion method, manages intensity cooling of Scars, ensures transactional atomicity, and handles symbol registration and governance metadata.
    *   **API Endpoint**: `POST /consensus`, `POST /consensus/bulk`, `GET /consensus/history`
    *   (Source: Kimera-SWM_v0.1_System_Spec.md, openapi.yaml)

*   **Output Translator**:
    *   **Function**: Provides human-readable representations of the system's state, including Geoids, Scar trails, diff reports, and resonance scores. Output can be in HTML or JSON.
    *   **API Endpoint**: `POST /translate` (or `GET /geoids/{id}?level=`)
    *   (Source: Kimera-SWM_v0.1_System_Spec.md)

## 4. Key Data Models

*   **Echoform**:
    *   **Description**: The atomic symbolic predicate of the system. It represents a fundamental piece of information extracted from the input data.
    *   **Key Attributes**: `id` (UUID), `symbol` (string), `vector` (array of numbers), `timestamp` (date-time).
    *   (Source: Kimera-SWM_v0.1_System_Spec.md, openapi.yaml, echoform.schema.json)

*   **Geoid**:
    *   **Description**: A cluster of Echoforms, organized along multiple axes (e.g., temporal, cultural, logical). Geoids have a rotation that is updated based on internal "tension."
    *   **Key Attributes**: `id` (UUID), `axes` (array of strings like temporal, cultural, logical), `rotation` (array of numbers), `echoform_ids` (array of UUIDs), `scar_ids` (array of UUIDs).
    *   (Source: Kimera-SWM_v0.1_System_Spec.md, openapi.yaml)

*   **Scar**:
    *   **Description**: A record of an event or an inference step (deduction, abduction, contradiction, consensus) that occurred within the system. Scars have an intensity and are associated with a specific axis.
    *   **Key Attributes**: `id` (UUID), `type` (enum: deduction, abduction, contradiction, consensus), `axis` (enum: temporal, cultural, logical), `intensity` (number), `timestamp` (date-time).
    *   (Source: Kimera-SWM_v0.1_System_Spec.md, openapi.yaml)

*   **SnapshotMeta**:
    *   **Description**: Metadata about a saved state (snapshot) of the Kimera-SWM system in the Vault.
    *   **Key Attributes**: `version_id` (UUID), `parent_id` (UUID, for lineage), `created` (date-time), `hash` (string, cryptographic hash of the snapshot).
    *   (Source: Kimera-SWM_v0.1_System_Spec.md, openapi.yaml)

*   **ConsensusEvent**:
    *   **Description**: Represents an event where the Consensus Engine has processed a set of Scars to produce a new insight or resolve a conflict.
    *   **Key Attributes**: `event_id` (UUID), `scar_ids` (array of UUIDs that were input to consensus), `new_scar_id` (UUID of the scar generated by this consensus), `axis` (enum), `intensity_map` (object), `timestamp` (date-time), `agent_id` (UUID, who triggered/approved), `rationale` (string).
    *   (Source: Kimera-SWM_v0.1_System_Spec.md, openapi.yaml)

## 5. Development Workflow

The Kimera-SWM project adheres to a structured development workflow, emphasizing a spec-first approach and robust testing.

(Source: Kimera-SWM_v0.1_System_Spec.md)

### 5.1 Spec-First Approach

Development begins with defining the system's contracts. All API endpoints and data models are first specified in `openapi.yaml` for the REST API and `kimeraswm.proto` for gRPC services. These specifications serve as the single source of truth for the system's interface.

(Source: Kimera-SWM_v0.1_System_Spec.md, Section 9)

## 7. Data Handling

The Kimera-SWM system processes data through a defined lifecycle, from initial ingestion to inference, storage, and conflict resolution. This ensures that information is handled consistently and that the system's memory remains auditable and coherent.

(Source: Kimera-SWM_v0.1_System_Spec.md, openapi.yaml)

### 7.1 Data Ingestion

Raw data enters the system through the **Intake Parser** module via the `POST /intake` API endpoint. This module accepts data in various formats such as `text`, `json`, or `csv`. The Intake Parser's primary role is to:
1.  Parse the input data.
2.  Transform it into **Echoforms**, which are atomic symbolic representations of information, each containing a symbol, a vector representation, and a timestamp.
3.  Normalize these vectors (unit ℓ²) and detect negations or specific patterns, which contribute to a "tension map" indicating areas of potential interest or conflict.
The output is a list of Echoforms and the associated tension map.

(Source: Kimera-SWM_v0.1_System_Spec.md, Sections 4, 5.1, 6; openapi.yaml, schemas/ParseRequest, schemas/ParseResponse, schemas/Echoform)

### 7.2 Data Processing and Clustering

The Echoforms generated by the Intake Parser are then processed by the **Geoid Constructor** module, typically invoked via the `POST /geoids/build` endpoint. This module:
1.  Receives a list of Echoforms.
2.  Clusters these Echoforms into **Geoids**. Geoids are multi-dimensional structures that group related Echoforms along various axes (e.g., temporal, cultural, logical). This clustering is based on a θ-threshold.
3.  Updates the "rotation" of these Geoids based on the tension scores derived during ingestion, reflecting the internal dynamics of the clustered data.
The output is a collection of Geoids.

(Source: Kimera-SWM_v0.1_System_Spec.md, Sections 4, 5.2, 6; openapi.yaml, schemas/BuildRequest, schemas/BuildResponse, schemas/Geoid)

### 7.3 Inference and Learning

Once data is structured into Geoids, the **Reactor Gate** module performs inference and learning. Invoked via `POST /reactor`, it takes Echoforms (often those within Geoids) and:
1.  Runs zetetic loops, which include:
    *   **Deduction**: Applying logical rules (Modus Ponens) to derive new conclusions.
    *   **Abduction**: Forming hypotheses based on vector similarity between Echoforms.
    *   **Contradiction**: Identifying conflicting information (φ & ¬φ) which triggers resolution processes.
2.  Logs every inference step and significant event as a **Scar**. Scars record the type of event (deduction, abduction, contradiction, consensus), the axis involved, its intensity, and a timestamp.
3.  If contradictions trigger rule changes, these are logged as **RuleMutations**.
4.  The Reactor Gate can also trigger micro-checkpoints to save the intermediate state.
The output includes the updated Geoids and a list of generated Scars. This iterative process is central to the system's "living-memory" characteristic.

(Source: Kimera-SWM_v0.1_System_Spec.md, Sections 1, 3, 4, 5.3, 6; openapi.yaml, schemas/ReactorRequest, schemas/ReactorResponse, schemas/Scar)

### 7.4 Data Storage and Auditing

The **Vault Module** is responsible for the persistence and auditability of the system's memory. Key functionalities include:
1.  **Snapshots (Checkpoints)**: The `POST /vault/checkpoint` endpoint allows the system to save its current state (Echoforms, Geoids, Scars, rules) as a cryptographically-signed, immutable snapshot. Each snapshot has a unique `version_id`, `parent_id` (for lineage), creation timestamp, and a hash.
2.  **Auditable Log**: Scars generated by the Reactor Gate serve as an indelible log of all operations, inferences, and changes within the system. This log, combined with versioned snapshots, provides full auditability.
3.  **State Management**: The Vault also handles reloading snapshots (`POST /vault/reload`), listing versions (`GET /vault/versions`), diffing between versions (`GET /vault/versions/{a}/diff/{b}`), and merging divergent states (`POST /vault/merge`).

This ensures that the system's memory is durable, verifiable, and can be rolled back or branched as needed.

(Source: Kimera-SWM_v0.1_System_Spec.md, Sections 1, 4, 5.4, 6; openapi.yaml)

### 7.5 Conflict Resolution

When contradictions are detected by the Reactor Gate and logged as Scars, or when different lines of reasoning produce conflicting Scars, the **Consensus Engine** module comes into play. Invoked via `POST /consensus`, it:
1.  Takes a set of `scar_ids` (often conflicting) as input, along with an `axis`, `agent_id` (for governance), and an optional `rationale`.
2.  Applies a weighted-average fusion logic to the input Scars.
3.  Generates a new **ConsensusEvent** and potentially a new Scar that represents the synthesized insight or resolved state.
4.  Manages the "intensity cooling" of Scars involved in the consensus.

This mechanism allows Kimera-SWM to be "contradiction-driven," using conflicts as opportunities to refine its understanding and generate new knowledge.

(Source: Kimera-SWM_v0.1_System_Spec.md, Sections 1, 4, 5.5, 6; openapi.yaml, schemas/ConsensusRequest, schemas/ConsensusEvent)

## 6. API Structure

The Kimera-SWM REST API provides endpoints for interacting with its various modules. The main endpoints are defined in `openapi.yaml`:

(Source: openapi.yaml)

*   **`/intake` (POST)**:
    *   **Purpose**: Parses raw input data (text, JSON, CSV) into Echoforms.
    *   **Operation ID**: `parseData`

*   **`/geoids/build` (POST)**:
    *   **Purpose**: Clusters a list of Echoforms into Geoids.
    *   **Operation ID**: `buildGeoids`

*   **`/reactor` (POST)**:
    *   **Purpose**: Runs inference cycles (deduction, abduction, contradiction) on Echoforms, producing updated Geoids and Scars.
    *   **Operation ID**: `runReactor`

*   **`/vault/checkpoint` (POST)**:
    *   **Purpose**: Creates a snapshot of the current memory state in the Vault.
    *   **Operation ID**: `checkpointMemory`

*   **`/vault/reload` (POST)**:
    *   **Purpose**: Reloads a previously saved snapshot from the Vault, applying per-axis decay.
    *   **Operation ID**: `reloadMemory`

*   **`/vault/versions` (GET)**:
    *   **Purpose**: Lists all available snapshot versions stored in the Vault.
    *   **Operation ID**: `listSnapshots`

*   **`/vault/versions/{a}/diff/{b}` (GET)**:
    *   **Purpose**: Computes and returns a diff report between two specified snapshot versions.
    *   **Operation ID**: `diffSnapshots`

*   **`/vault/merge` (POST)**:
    *   **Purpose**: Merges two specified snapshots using a weighted union, creating a new snapshot.
    *   **Operation ID**: `mergeSnapshots`

*   **`/consensus` (POST)**:
    *   **Purpose**: Performs a single consensus fusion operation on a set of specified Scars.
    *   **Operation ID**: `addConsensus`

### 5.2 Code Generation

Once the specifications are defined, code generation tools are employed to create boilerplate code. OpenAPI-Generator is used to scaffold API controllers and data models from `openapi.yaml`, while `protoc` (protobuf compiler) is used for generating code from `.proto` files for gRPC. This ensures that the implementation is consistent with the defined contracts.

(Source: Kimera-SWM_v0.1_System_Spec.md, Section 9)

### 5.3 Testing Strategy

A comprehensive testing strategy is in place to ensure system reliability and correctness:

*   **Contract Tests**: Tools like Schemathesis or Dredd are used to validate that the implemented API endpoints conform strictly to the `openapi.yaml` specification.
*   **Unit Tests**: These focus on testing individual components and functions in isolation, such as fusion mathematics, decay formulas, clustering algorithms, and data normalization logic. AI-assisted scaffolding is mentioned for generating these tests.
*   **Integration Tests**: These tests verify the interaction between different modules, ensuring that data flows correctly through the pipeline (e.g., Intake → Geoids → Reactor → Vault → Merge → Consensus → Reactor loops).
*   **Performance Tests**: Service level objectives (SLOs) are defined with p95 latency budgets for critical operations (e.g., 50ms for consensus, 100ms for reactor, 300ms for end-to-end operations).
*   **Chaos Tests**: The system is subjected to various failure scenarios to test its resilience, including mid-transaction crashes, high-load concurrency, oscillation stress, and data corruption.

(Source: Kimera-SWM_v0.1_System_Spec.md, Sections 9 & 10)

### 5.4 CI/CD Pipeline

A Continuous Integration / Continuous Deployment (CI/CD) pipeline automates the build, test, and deployment process. The typical stages are:

1.  **Validate Spec**: Ensures the API specifications (`openapi.yaml`, `.proto`) are well-formed and valid.
2.  **Code Gen**: Automatically generates server stubs, client libraries, and data models from the specifications.
3.  **Contract Tests**: Runs tests to verify that the API implementation adheres to its contract.
4.  **Build**: Compiles the source code and packages the application.
5.  **Unit/Integration Tests**: Executes automated unit and integration tests to check business logic and component interactions.
6.  **Deploy**: Deploys the application to the target environment.

(Source: Kimera-SWM_v0.1_System_Spec.md, Section 9)
