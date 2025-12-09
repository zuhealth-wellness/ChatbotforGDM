# GraphRAG Architecture for Gestational Diabetes Mellitus Research Chatbot

## Overview

This project implements a **GraphRAG (Graph Retrieval-Augmented Generation)** architecture for querying medical research articles on Gestational Diabetes Mellitus (GDM). The system combines knowledge graph construction with hybrid retrieval to provide evidence-based answers from a corpus of research papers spanning 2000-2024.

**Citation:**
F. Ruba, A. Nazir, E. Evangelista, S. Bukhari, L. bin Mohd Lofti & R. Sharma. (2025.) Data Repository for GraphRAG-Architecture-of-a-local-LLM-for-Gestational-Diabetes-Mellitus. Available at: https://github.com/zuhealth-wellness/GDMChatbot.

---

## Version 1: Research Prototype

**Version 1** (used in our research article) was a proof-of-concept implementation built on **Google Colab** using:

- **Neo4j Database**: Cloud-hosted graph database for knowledge graph storage
- **Semantic Scholar API**: Automated data collection from research articles (2000-2024)
- **OpenAI GPT-3.5 Turbo**: Entity extraction and relationship identification
- **Monolithic Notebook Architecture**: Single Jupyter notebook workflow

The V1 prototype successfully demonstrated the GraphRAG concept, extracting entities and relationships from PDF research papers and constructing a knowledge graph. It validated the hybrid retrieval approach combining structured graph traversal with unstructured vector search.

---

## Why Version 2?

While V1 proved the concept, several limitations necessitated a production-ready architecture:

1. **Scalability**: Neo4j cloud instances had connection limits and cost constraints for production deployment
2. **Deployment**: Colab notebooks are not suitable for persistent, accessible services
3. **Architecture**: Monolithic design limited scalability and maintainability
4. **Database Portability**: Platform-specific database formats complicated cross-platform deployment
5. **User Experience**: No persistent web interface for end-user interaction

**Version 2** addresses these limitations with a production-grade, client-server architecture using **Kuzu** (embedded graph database), **FastAPI** backend, and **Chainlit** frontend, enabling scalable deployment and better user experience.

---

## Version 2: Core Architecture

### System Overview

```
User Question → Entity Extraction → Hybrid Retrieval → LLM Generation → Answer
                     ↓                      ↓
              [Structured]          [Unstructured]
         Graph Traversal (Kuzu)    Vector Search (Embeddings)
```

### Key Components

1. **Knowledge Graph Database (Kuzu)**
   - Embedded graph database with 159,314 nodes
   - Stores entities (Interventions, Outcomes, Populations, etc.) and relationships
   - Platform-portable database format

2. **Hybrid Retrieval System**
   - **Structured Retrieval**: Graph traversal to find entity co-mentions and relationships
   - **Unstructured Retrieval**: Vector similarity search on document chunks
   - **Hybrid Fusion**: Combines both retrieval methods for comprehensive context

3. **Multi-Model LLM Support**
   - **Cloud Models**: OpenAI (GPT-4o, GPT-3.5), Groq (Llama 3.3 70B), Google Gemini
   - **Local Models**: Llama/Mistral GGUF models for privacy-sensitive deployments
   - **Intelligent Routing**: Automatic fallback chain based on availability and context requirements

4. **Client-Server Architecture**
   - **FastAPI Backend**: RESTful API for RAG pipeline (Port 8001)
   - **Chainlit Frontend**: ChatGPT-like web interface (Port 8000)
   - **Separation of Concerns**: Scalable, maintainable, production-ready

---

## Key Features

- **Evidence-Based Answers**: Citations from research papers with source attribution
- **Hybrid Retrieval**: Combines graph structure and semantic search for comprehensive context
- **Multi-Model Flexibility**: Support for cloud and local LLMs with automatic context management
- **Production Architecture**: FastAPI + Chainlit for scalable deployment
- **Medical Disclaimer**: Built-in safety disclaimers on all responses
- **Thread-Safe Operations**: Database singleton pattern for concurrent access

---

## Quick Start

### Prerequisites

- Python 3.11+
- Kuzu graph database (159,314 nodes) - located at `../gdm_new` or set `DB_PATH` in `.env`
- OpenAI API key (required for embeddings)

### Installation

```bash
# Clone repository
git clone <repository-url>
cd gdm_app_v2

# Install dependencies
pip install -r requirements.txt
```

### Configuration

Create `.env` file:

```env
OPENAI_API_KEY=your_key_here
DB_PATH=../gdm_new
API_URL=http://localhost:8001/chat
```

### Running the Application

**Terminal 1 - Start Backend:**
```bash
python api.py
```

**Terminal 2 - Start Frontend:**
```bash
chainlit run ui.py
```

Access the application at `http://localhost:8000`

---

## Project Structure

```
gdm_app_v2/
├── api.py              # FastAPI backend (REST API)
├── ui.py               # Chainlit frontend (Web UI)
├── query.py            # Core RAG pipeline (Hybrid retrieval)
├── model_router.py     # Multi-tier LLM routing
├── local_llm.py        # Local model wrapper
├── EntityExtKG.py      # Database schema & initialization
├── config.py           # Configuration helper
├── import_data.py      # Database migration utility
└── requirements.txt    # Python dependencies
```

---

## How It Works

1. **Question Processing**: User question is analyzed to extract medical entities (interventions, outcomes, populations)

2. **Hybrid Retrieval**:
   - **Structured**: Graph queries find entities and their relationships (co-mentions, connections)
   - **Unstructured**: Vector search finds semantically similar document chunks
   - **Fusion**: Both contexts are combined with structured evidence prioritized

3. **Answer Generation**: LLM synthesizes retrieved context into an evidence-based answer with inline citations

4. **Response**: Answer includes source citations, allowing users to verify information from original research papers

---

## Technical Stack

- **Graph Database**: Kuzu (embedded, platform-portable)
- **RAG Framework**: LangChain with custom hybrid retrieval
- **Backend**: FastAPI (async, high-performance)
- **Frontend**: Chainlit (ChatGPT-like interface)
- **Embeddings**: OpenAI text-embedding-3-small
- **LLMs**: Multi-provider support (OpenAI, Groq, Gemini, Local GGUF)

---

## Status

**Version 2.0** is currently in **deployment phase**. The system has been successfully migrated from the research prototype (V1) to a production-ready architecture. Beta testing and deployment infrastructure setup are underway.

---

**Version**: 2.0.0  
**Last Updated**: 2024
