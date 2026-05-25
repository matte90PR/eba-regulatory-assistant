# EBA Regulatory Assistant

A RAG-based conversational assistant for querying EBA (European Banking Authority) regulatory guidelines using LangChain, ChromaDB, and OpenAI embeddings.

## Overview

This project implements a Retrieval-Augmented Generation (RAG) pipeline that allows users to ask questions about EBA regulatory documents in natural language. The assistant retrieves relevant passages from the guidelines and generates accurate, cited answers.

Built as part of a study path toward becoming an AI Transformation Architect in the banking/regulatory domain.

## Architecture

```
PDF Documents (EBA Guidelines)
        │
        ▼
PyPDFLoader + Metadata enrichment
        │
        ▼
RecursiveCharacterTextSplitter (chunk_size=1500, overlap=225)
        │
        ▼
OpenAI Embeddings (text-embedding-3-small)
        │
        ▼
ChromaDB Vector Store (persistent)
        │
        ▼
MMR Retriever (k=5, fetch_k=10, lambda_mult=0.7)
        │
        ▼
ConversationalRetrievalChain + ConversationSummaryBufferMemory
        │
        ▼
Groq LLM (llama-3.3-70b-versatile)
        │
        ▼
Answer with page citations
```

## Features

- **Multi-chunk RAG** — loads and indexes EBA PDF guidelines with rich metadata (page, section, year, topic)
- **MMR Retrieval** — Maximal Marginal Relevance ensures diverse, non-redundant chunks are retrieved
- **Conversational memory** — `ConversationSummaryBufferMemory` maintains context across follow-up questions
- **Page citations** — every answer references the source page from the original document
- **Noise filtering** — short chunks (headers, page numbers) are filtered before indexing
- **SSL proxy support** — compatible with corporate network environments

## Tech Stack

| Component | Technology |
|---|---|
| Framework | LangChain |
| LLM | Groq (llama-3.3-70b-versatile) — free |
| Embeddings | OpenAI text-embedding-3-small |
| Vector Store | ChromaDB (persistent) |
| Document Loader | PyPDFLoader |
| Memory | ConversationSummaryBufferMemory |

## Getting Started

### Prerequisites

- Python 3.11+
- Conda (recommended)
- OpenAI API key
- Groq API key (free at [console.groq.com](https://console.groq.com))

### Installation

```bash
# Clone the repository
git clone https://github.com/mattez90/eba-regulatory-assistant.git
cd eba-regulatory-assistant

# Create and activate conda environment
conda create -n eba-rag python=3.11
conda activate eba-rag

# Install dependencies
pip install -r requirements.txt
```

### Configuration

Create a `.env` file in the project root:

```
GROQ_API_KEY=your_groq_api_key
OPENAI_API_KEY=your_openai_api_key
```

### Usage

1. Place your EBA Guidelines PDF in the `00 Data/` folder
2. Open `01 Week 1/EsercizioRAG.ipynb` in JupyterLab
3. Run all cells in order
4. Use the conversational interface in Section 5 to query the documents

```bash
conda activate eba-rag
cd "path/to/eba-regulatory-assistant"
jupyter lab
```

## Project Structure

```
eba-regulatory-assistant/
├── 00 Data/                    # EBA Guidelines PDFs (not tracked)
├── 01 Week 1/
│   └── EsercizioRAG.ipynb     # Main RAG notebook
├── chroma_db/                  # Vector store (not tracked)
├── .env                        # API keys (not tracked)
├── .gitignore
├── requirements.txt
└── README.md
```

## Data Sources

All documents used are publicly available:

- [EBA Guidelines on PD/LGD estimation](https://www.eba.europa.eu/publications-and-media/publications/guidelines)
- [EBA Consultation Paper on CCF Guidelines](https://www.eba.europa.eu/publications-and-media/publications/guidelines)

No proprietary or confidential data is used in this project.

## Roadmap

- [x] PDF loading with metadata enrichment
- [x] Recursive text splitting with noise filtering
- [x] OpenAI embeddings + ChromaDB vector store
- [x] MMR retrieval
- [x] Conversational memory
- [ ] Tool use — web search (Tavily) for recent regulatory updates
- [ ] Multi-document support (3-5 EBA guidelines simultaneously)
- [ ] Gap report generator — compare EBA guidelines vs internal policy
- [ ] Streamlit UI

## Author

**mattez90** — Senior Credit Risk Manager transitioning to AI Transformation Architect

---

*Built with LangChain · Groq · OpenAI · ChromaDB*
