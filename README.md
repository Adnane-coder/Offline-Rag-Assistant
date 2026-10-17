# Offline RAG Assistant

A local Retrieval-Augmented Generation (RAG) API built with **FastAPI** and **ChromaDB**. It runs fully offline: no external API calls, no usage costs, and your documents never leave your machine.

> Built while following the NextWork "Build a RAG API with FastAPI" project, then extended and documented as my own version.

## Features

- Ingests documents, splits them into chunks, and stores their embeddings in ChromaDB
- Retrieves the most relevant chunks for a given question
- Generates an answer grounded in the retrieved context using a local LLM
- Exposes everything through a simple REST API with automatic Swagger docs
- Works entirely offline once models are downloaded

## How it works

```
Documents ──► Chunking ──► Embeddings ──► ChromaDB (vector store)
                                               │
User question ──► Embedding ──► Similarity search
                                               │
                                  Top-k relevant chunks
                                               │
                              Prompt (question + context)
                                               │
                                          Local LLM
                                               │
                                     Grounded answer (JSON)
```

## Tech stack

| Component | Tool |
|---|---|
| API framework | FastAPI |
| Vector database | ChromaDB |
| LLM runtime | Local model (e.g. via Ollama) |
| Language | Python 3.10+ |

## Getting started

### 1. Clone the repository

```bash
# Local terminal
git clone https://github.com/<your-username>/offline-rag-assistant.git
cd offline-rag-assistant
```

### 2. Create a virtual environment and install dependencies

```bash
# Local terminal, inside the project folder
python -m venv venv
source venv/bin/activate        # Windows PowerShell: venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

### 3. Set up the local model

Make sure your local LLM runtime is installed and the model you use is downloaded. Example with Ollama:

```bash
# Local terminal
ollama pull <model-name>
```

### 4. Run the API

```bash
# Local terminal, inside the project folder, venv activated
uvicorn main:app --reload
```

The API is now available at `http://127.0.0.1:8000`, and the interactive docs at `http://127.0.0.1:8000/docs`.

## Usage

Example request (adjust the endpoint and fields to match your API):

```bash
# Local terminal
curl -X POST "http://127.0.0.1:8000/query" \
  -H "Content-Type: application/json" \
  -d '{"question": "Your question here"}'
```

Example response:

```json
{
  "answer": "Generated answer based on the retrieved context.",
  "sources": ["chunk 1", "chunk 2"]
}
```

## Project structure

```
offline-rag-assistant/
├── main.py              # FastAPI app and endpoints
├── requirements.txt     # Python dependencies
├── data/                # Source documents
├── chroma_db/           # Vector store (ignored by git)
└── README.md
```

Update this section to match your actual folder layout.

## Roadmap

- [ ] Evaluation set (questions with expected answers) and retrieval metrics (hit rate, MRR)
- [ ] Chunking experiments (size, overlap) with before/after results
- [ ] Reranking and hybrid search (BM25 + embeddings)
- [ ] Dockerize the application
- [ ] Support for more document formats (PDF, DOCX)

## Credits

Based on the NextWork project guide "Build a RAG API with FastAPI". Extended and documented by Adnane Ben Ammar.

## Screenshoot(SWAGGER)
<img width="1911" height="962" alt="first one" src="https://github.com/user-attachments/assets/872bdd51-13fa-4e75-91dd-8712d4891706" />

<img width="1918" height="972" alt="addition-information" src="https://github.com/user-attachments/assets/315d3694-bbf5-4b98-b8cf-f73cdb0b570f" />

<img width="1918" height="970" alt="answer" src="https://github.com/user-attachments/assets/f857cb21-a03d-4fe7-a9da-c2ae0c37c1e4" />

## License

Adnane Ben Ammar EPI Digiral school Student
