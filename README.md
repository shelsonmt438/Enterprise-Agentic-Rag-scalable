# Enterprise Agentic RAG (Scalable Pipeline)



A production-grade, cyclic RAG system built with **LangGraph**, **Google Cloud Platform (GCP)**, and **Groq**. This system distinguishes between technical "True Data" and random "Noisy Data" using semantic re-ranking and history-aware planning.



## Key Features

*   **Agentic Intelligence**: Uses LangGraph for complex, cyclic reasoning and multi-step planning.

*   **Enterprise Search**: Integration with **Qdrant Cloud** for high-performance vector search and **FlashRank** for ultra-fast local semantic reranking.

*   **Observability**: Full tracing with **Pydantic Logfire** and **LangSmith** for real-time monitoring of agent decisions.

*   **Scalable Infrastructure**: Deployed on **Google Cloud Run** with **Cloud Build** CI/CD and **VPC Connectors** for private networking.

*   **Document Intelligence**: Uses **Google Document AI** for high-fidelity PDF parsing and data extraction.

---

## 🔄 Agent Intelligence Flow

```mermaid
%%{init: {'theme': 'dark', 'themeVariables': { 'primaryColor': '#3182ce', 'edgeLabelBackground': '#1a202c', 'tertiaryColor': '#2d3748' }}}%%
graph TD
    User((User)) --> UI[Streamlit UI]
    UI --> Planner{Planner Node}
    Planner -->|Conversational| Responder[Responder Node]
    Planner -->|Technical| Retriever[Retriever Node]
    Retriever --> Reranker[FlashRank Local Reranker]
    Reranker --> Responder
    Responder --> UI
    Responder -.-> Memory[(LangGraph MemorySaver)]

## 📂 Project Structure
```text
├── app/
│   ├── agents/          # LangGraph Nodes, State, and Graph compilation
│   ├── config.py        # Centralized environment variable management
│   ├── ingestion/       # End-to-end data processing (Loaders, Chunking)
│   ├── main.py          # FastAPI application entrypoint
│   ├── services/
│   │   ├── retrieval/   # Vector search (Qdrant), Embeddings (Vertex), Ranking (FlashRank)
│   │   └── storage/     # Postgres Checkpointers and Redis Cache
├── ui/                  # Streamlit interface with source transparency & reasoning steps
├── DOCS/                # Comprehensive architectural and operational documentation
├── DATA/                # Sample datasets (True vs Noisy documentation)
├── Dockerfile           # Optimized production container definition
└── requirements.txt     # Locked dependencies for local and cloud parity
```

---

## 🏗️ Tech Stack
*   **Orchestration**: LangChain & LangGraph
*   **LLMs**: Groq (Llama 3.3 70B) for lightning-fast reasoning
*   **Vector DB**: Qdrant (Cloud)
*   **Cloud Platform**: Google Cloud Platform
    *   **Compute**: Cloud Run (Serverless)
    *   **Storage**: Cloud Storage (GCS)
    *   **Database**: Cloud SQL (Postgres) & Redis
    *   **AI Services**: Vertex AI (Embeddings), FlashRank (Local Reranking), Document AI
*   **Observability**: Logfire (OpenTelemetry) & LangSmith

---


---

## 🛠️ Getting Started

1.  **Clone & Install**:
    ```bash
    pip install -r requirements.txt
    ```
2.  **Configure**:
    Copy `commands.md.example` to `commands.md` and fill in your keys.
3.  **Run Ingestion**:
    ```bash
    python -m app.ingestion.processor DATA --wipe
    ```
4.  **Launch App**:
    ```bash
    uvicorn app.main:app --port 8000 & streamlit run ui/app.py
    ```

---

