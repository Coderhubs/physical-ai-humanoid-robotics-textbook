# Feature: FastAPI RAG Chatbot

## Phase 1: Setup

- [ ] T001 Create base project directory structure for FastAPI application
- [ ] T002 Initialize `uv` project with `pyproject.toml` and specified dependencies
- [ ] T003 Configure `python-dotenv` for environment variable management
- [ ] T004 Create `alembic.ini` and `env.py` for Alembic migrations
- [ ] T005 Initialize Alembic environment in `migrations/`

## Phase 2: Foundational

- [ ] T006 [P] Implement base FastAPI application in `src/main.py`
- [ ] T007 [P] Define `settings.py` using `pydantic-settings` for application configuration
- [ ] T008 [P] Configure database connection with SQLModel in `src/database.py`
- [ ] T009 [P] Create initial Alembic migration script for database setup

## Phase 3: User Story 1: Chatbot Core Functionality [US1]

**Goal**: Establish the basic FastAPI chat endpoint with an OpenAI Agent and streaming responses.

- [ ] T010 [US1] Define `AgentExecutor` instance in `src/agent.py`
- [ ] T011 [US1] Implement streaming response logic for `AgentExecutor` in `src/agent.py`
- [ ] T012 [US1] Create `POST /chat` endpoint in `src/main.py` for basic chat interaction
- [ ] T013 [US1] Integrate `AgentExecutor` with `POST /chat` endpoint in `src/main.py`

## Phase 4: User Story 2: Qdrant RAG Integration [US2]

**Goal**: Develop a custom RAG tool that queries Qdrant Cloud for context, and ensure the agent uses it when appropriate.

- [ ] T014 [US2] Implement `QdrantClient` initialization in `src/rag_tool.py`
- [ ] T015 [US2] Create embedding model initialization using `sentence-transformers` in `src/embeddings.py`
- [ ] T016 [US2] Develop `search_qdrant` function that queries Qdrant Cloud (`top-k=5`) and formats results in `src/rag_tool.py`
- [ ] T017 [US2] Wrap `search_qdrant` as a custom `OpenAI Agents SDK` tool in `src/rag_tool.py`
- [ ] T018 [US2] Add the custom RAG tool to the `AgentExecutor` definition in `src/agent.py`
- [ ] T019 [US2] Implement document indexing functionality using `pymupdf` and `Qdrant` client in `src/document_processor.py`
- [ ] T020 [US2] Create an endpoint `POST /upload` for document ingestion and indexing in `src/main.py`

## Phase 5: User Story 3: Chat Modes Implementation [US3]

**Goal**: Implement "full" and "selection" chat modes in the `POST /chat` endpoint.

- [ ] T021 [US3] Modify `POST /chat` endpoint in `src/main.py` to accept a `mode` parameter
- [ ] T022 [US3] Implement `mode="full"` logic, allowing agentic RAG, in `src/main.py`
- [ ] T023 [US3] Implement `mode="selection"` logic, injecting `selected_text` and disabling RAG tool, in `src/main.py`

## Phase 6: User Story 4: Persistence with Neon Postgres [US4]

**Goal**: Store chat history and other relevant data in a Neon Postgres database using SQLModel and Alembic.

- [ ] T024 [US4] Define `ChatMessage` SQLModel in `src/models/chat.py` (e.g., `id`, `session_id`, `role`, `content`, `timestamp`)
- [ ] T025 [US4] Define `Document` SQLModel in `src/models/document.py` (e.g., `id`, `filename`, `content_hash`, `upload_timestamp`)
- [ ] T026 [US4] Generate new Alembic migration script for `ChatMessage` and `Document` models
- [ ] T027 [US4] Implement CRUD operations for `ChatMessage` in `src/services/chat_history.py`
- [ ] T028 [US4] Integrate `chat_history` service into `POST /chat` endpoint for saving messages in `src/main.py`
- [ ] T029 [US4] Implement CRUD operations for `Document` in `src/services/document_store.py`
- [ ] T030 [US4] Integrate `document_store` service into `POST /upload` endpoint for saving document metadata in `src/main.py`

## Phase 7: Polish & Cross-Cutting Concerns

- [ ] T031 Add error handling and logging across the application
- [ ] T032 Implement basic authentication/authorization for API endpoints (if not covered by an explicit US)
- [ ] T033 Create comprehensive `README.md` with setup, usage, and API documentation
- [ ] T034 Review and refine `pyproject.toml` for final dependency management and project metadata

## Dependencies

- Phase 1 must complete before Phase 2.
- Phase 2 must complete before Phase 3, Phase 4, Phase 5, and Phase 6.
- Phase 3, Phase 4, Phase 5, and Phase 6 can be developed largely in parallel after Foundational tasks, but US4 (Persistence) might be beneficial to start early for data models.
- Phase 7 depends on all previous phases.

## Parallel Execution Examples

### User Story 1: Chatbot Core Functionality
- T010, T011, T012, T013 can be developed sequentially, but aspects of T010 (agent setup) could be done alongside T012 (endpoint definition).

### User Story 2: Qdrant RAG Integration
- T014, T015, T016 can be done in parallel as they set up independent components (Qdrant client, embeddings, search function). T017 depends on T016, and T018 depends on T017. T019 and T020 are dependent on the Qdrant setup but can run somewhat in parallel with the agent integration.

### User Story 3: Chat Modes Implementation
- T021, T022, T023 can be worked on sequentially within the `POST /chat` endpoint.

### User Story 4: Persistence with Neon Postgres
- T024 and T025 can be done in parallel as they define independent SQLModels. T026 depends on these. T027 and T029 can be done in parallel after models are defined. T028 and T030 depend on their respective services.

## Implementation Strategy

The implementation will follow an MVP-first approach, iteratively building out features.

1.  **Core Chatbot (US1):** Focus on getting a basic, streaming chat interaction working with the OpenAI Agent.
2.  **RAG Integration (US2):** Integrate Qdrant and the custom RAG tool, ensuring the agent intelligently uses it.
3.  **Chat Modes (US3):** Add the "full" and "selection" modes to the chat endpoint.
4.  **Persistence (US4):** Implement data models and services for chat history and document storage.
5.  **Refinement:** Address error handling, logging, and documentation.

## Report

- tasks.md generated successfully.
- Total tasks: 34
- Tasks per user story:
    - Setup: 5
    - Foundational: 4
    - US1: Chatbot Core Functionality: 4
    - US2: Qdrant RAG Integration: 7
    - US3: Chat Modes Implementation: 3
    - US4: Persistence with Neon Postgres: 7
    - Polish & Cross-Cutting Concerns: 4
- Parallel opportunities identified within and across user stories.
- Independent test criteria for each story will be derived during implementation.
- Suggested MVP scope: Phase 1, Phase 2, and Phase 3 (Chatbot Core Functionality).
- All tasks follow the checklist format.