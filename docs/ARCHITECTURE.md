# Architecture

EchoLite surrounds a local language model with persistent state, bounded retrieval and tools. Ernos Plain is the implementation language for the application, not a separate reasoning mechanism.

## Per-turn context

A turn begins with a frozen revision of `Me.txt`, relevant retrieved records, active working state and recent conversation. Tool results extend that turn's evidence. An identity edit is recorded immediately but becomes active on the next turn, so one action cycle has a consistent identity revision.

The configured model capacity is 262,144 tokens. Retrieval and bounded excerpts control what is useful to include; the system does not insert its entire disk history merely because the context window is large.

## Layers

| Layer | Role |
| --- | --- |
| Identity | Versioned `Me.txt`, capped at 1,200 actual model tokens; editable and inspectable |
| Working | Tasks, next actions, blockers and completion evidence |
| Episodic | Conversations, tool events, interruptions and outcomes |
| Semantic | Retained claims with provenance, belief states and validity bounds |
| Procedural | Remembered methods, with the same need for evidence and revision |
| Reference | Local documents and knowledge snapshots, distinct from autobiographical experience |

Local MiniLM embeddings supply passive recall; ranking and source windows are bounded. Exact lexical lookup remains a diagnostic path, not the normal substitute for embedding recall. Retrieval itself does not establish truth or usefulness.

## Belief, storage and time are separate

A note can be active or archived independently of whether its claim is tentative, supported, disputed, superseded or expired. Assessments use revision checks and retain history without rewriting the original text. Supported claims point to retained user statements or successful source/calculation/execution observations; the attribution is not a proof of semantic entailment.

Known validity dates are explicit UTC bounds. Unknown dates remain unknown. Expiry changes current retrieval eligibility and its cache revision; unchanged embedding vectors remain reusable. Historical records can still be deliberately opened.

## Tools and action

The model selects structured actions. EP validates arguments, records requests and observations, and distinguishes execution receipts from intentions. Tool families are exposed on demand so every turn need not carry every detailed schema.

Process calls use literal argument arrays and bounded output. Durable journals support output retrieval and deliberate retries. A failed tool, a task record or a confident paragraph cannot by itself complete a tracked task. Scheduling uses persisted one-shot triggers and respects active interaction.

## Caching and forgetting

Caching is persistent where valid: source results, embeddings, retrieval results, exact generation results and a managed native model-state snapshot. Keys include relevant identity/version/epoch information. Changing evidence or forgetting invalidates affected results. A live browser action is not silently replayed as if an old receipt were a fresh observation.

Consolidation is explicitly lossy and linked to retained sources. Corrections supersede old records. Forgetting follows dependencies, invalidates caches and removes corresponding vectors/metadata. Backup restore has recovery and forgetting-generation checks to reduce accidental resurrection.

## Native boundaries

- EP owns the UI, conversation controller, storage, tool dispatch, lifecycle and tests.
- llama.cpp supplies generation, embeddings and vision inference.
- Raylib supplies graphics; native macOS frameworks support images and documents.
- Kiwix supplies archive serving; GMP supplies exact rational arithmetic.

These are separately licensed native dependencies. They do not make the authored EchoLite application a Python service.
