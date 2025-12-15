# AtlasRAG-Gemini

End-to-end RAG (Retrieval-Augmented Generation) prototype that ingests a PDF, chunks it, generates local embeddings with **BAAI/bge-m3 (1024-dim)**, stores vectors in **MongoDB Atlas Vector Search**, retrieves top-k semantically relevant chunks, and produces grounded answers using **Gemini (LangChain)**.

## Architecture

1. **Ingestion**
   - Load PDF (PyPDFLoader)
   - Split into chunks (RecursiveCharacterTextSplitter)

2. **Embedding**
   - Local embeddings via SentenceTransformers: `BAAI/bge-m3`
   - Normalized vectors for cosine similarity

3. **Storage + Index**
   - Store `{ text, embedding }` in MongoDB collection
   - Create Atlas Vector Search index (`numDimensions=1024`, `similarity=cosine`)
   - Poll until index becomes queryable

4. **Retrieval**
   - `$vectorSearch` aggregation to fetch top-k chunks + scores

5. **Generation**
   - Prompt with retrieved context
   - Answer using Gemini via `ChatGoogleGenerativeAI`

## Tech Stack

- Python
- MongoDB Atlas + Vector Search
- LangChain (`langchain-community`, `langchain-text-splitters`, `langchain-google-genai`)
- SentenceTransformers (`BAAI/bge-m3`)
- Gemini API

## Setup

### 1) Create a virtual environment

```bash
python -m venv venv
# Windows:
venv\Scripts\activate
# macOS/Linux:
source venv/bin/activate
