# RAG App

A lightweight retrieval-augmented generation (RAG) application built with FastAPI, ChromaDB, and Ollama. The app stores document chunks in a local vector database and answers questions using the most relevant context retrieved for the given user or across all users.

This project is still a work in progress. Core functionality is in place, but features such as PDF ingestion, authentication, and several additional product improvements are still pending.

## What it does right now

- Accepts user-specific document text through a FastAPI endpoint
- Splits the content into paragraph-based chunks
- Stores those chunks in a persistent ChromaDB collection
- Uses Ollama embeddings to index the stored text
- Retrieves the most relevant chunks for a question
- Builds a context-augmented prompt and sends it to a local LLM
- Returns the final answer, along with the context used during retrieval

## Current API

### POST /documents

Adds document chunks for a user.

Request body:

```json
{
  "user_name": "alice",
  "content": "Alice is a product manager...\n\nAlice enjoys building ..."
}
```

Response:

```json
{
  "message": "Added 2 chunks for user 'alice'.",
  "user_name": "alice",
  "chunks_added": 2
}
```

### GET /ask

Asks a question against the stored document chunks.

Example:

```http
GET /ask?question=What does Alice do?&user=alice
```

Response includes:

- the answer from the LLM
- the retrieved context chunks
- the user filter used during the query

## Tech stack

- Python
- FastAPI
- Pydantic
- ChromaDB
- Ollama
- Local embedding model via `nomic-embed-text` (configurable)
- Local chat model via `qwen2.5:0.5b` (current implementation)

## Configuration

The app reads environment variables from a `.env` file.

Example:

```env
MODEL_NAME=nomic-embed-text
LLM_URL=http://localhost:11434
```

The default embedding model is `nomic-embed-text`, and the default Ollama URL is `http://localhost:11434`.

## How to run

1. Start Ollama locally.
2. Make sure the required model is available in Ollama.
3. Install the dependencies:

```bash
pip install fastapi pydantic ollama chromadb python-dotenv
```

4. Run the app:

```bash
uvicorn main:app --reload
```

The app uses a persistent ChromaDB directory at `./chroma_db`.

## Roadmap / pending work

This repo is still under active development. Planned and pending work includes:

- PDF ingestion and document upload support
- Better document parsing and chunking strategies
- User authentication and authorization
- Multi-user / multi-tenant improvements
- Admin and document management features
- Improved prompts and retrieval tuning
- UI or frontend integration
- Deployment and environment hardening

## Notes

This is a minimal RAG prototype intended to show the flow of:

- indexing text into a vector database
- retrieving context based on a user query
- augmenting an LLM prompt with relevant context
- returning a grounded answer from local models

It is not yet production-ready, and more features are planned as the project matures.
