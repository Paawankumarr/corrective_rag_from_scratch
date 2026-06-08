# Corrective RAG (CRAG)

A production-ready implementation of **Corrective Retrieval-Augmented Generation (CRAG)** using **LangGraph**, **LangChain**, **OpenAI**, and **Pydantic**. This system improves traditional RAG pipelines by evaluating retrieved documents and dynamically correcting retrieval failures through web search and hybrid retrieval strategies.

## Overview

Traditional RAG systems rely heavily on the quality of retrieved documents. When retrieval returns incomplete, irrelevant, or ambiguous information, the generated answer may be inaccurate.

This project implements a **Corrective RAG (CRAG)** workflow that:

* Evaluates the relevance of retrieved documents.
* Uses retrieved context directly when it is sufficient.
* Performs web search when retrieved information is incorrect or insufficient.
* Combines internal knowledge and external search results when ambiguity is detected.
* Produces more reliable and grounded responses.

## Features

* Document ingestion and vector storage
* Semantic search and retrieval
* Retrieval quality assessment
* Query routing based on retrieval confidence
* Web search integration for retrieval correction
* Hybrid retrieval for ambiguous queries
* Structured outputs using Pydantic
* Agentic workflow orchestration using LangGraph
* Production-ready modular architecture

## Architecture

```text
User Query
     │
     ▼
Document Retriever
     │
     ▼
Document Evaluator
     │
 ┌───┼───────────────┐
 │   │               │
 ▼   ▼               ▼
Correct Ambiguous   Incorrect
 │      │             │
 │      ▼             ▼
 │  Hybrid Search   Web Search
 │  (Docs + Web)      │
 │      │             │
 └──────┴─────────────┘
            │
            ▼
     Response Generator
            │
            ▼
       Final Answer
```

## Decision Logic

### Case 1: Correct Retrieval

If retrieved documents contain sufficient information:

```text
Query → Retrieve → Evaluate → Generate Answer
```

No additional retrieval operations are performed.

### Case 2: Incorrect Retrieval

If retrieved content is irrelevant or insufficient:

```text
Query → Retrieve → Evaluate → Web Search → Generate Answer
```

External information is fetched to improve answer quality.

### Case 3: Ambiguous Retrieval

If retrieved documents are partially relevant:

```text
Query → Retrieve → Evaluate
              │
              ▼
      Documents + Web Search
              │
              ▼
       Combined Context
              │
              ▼
        Generate Answer
```

The system merges internal and external knowledge sources.

## Technology Stack

| Component              | Technology        |
| ---------------------- | ----------------- |
| Workflow Orchestration | LangGraph         |
| LLM Framework          | LangChain         |
| Language Model         | OpenAI GPT Models |
| Data Validation        | Pydantic          |
| Vector Database        | FAISS / Chroma    |
| Embeddings             | OpenAI Embeddings |
| Web Search             | Tavily Search API |
| Programming Language   | Python            |

## Project Structure

```text
crag/
│
├── app/
│   ├── graph.py
│   ├── nodes.py
│   ├── state.py
│   ├── retriever.py
│   ├── evaluator.py
│   ├── web_search.py
│   └── generator.py
│
├── data/
│   └── documents/
│
├── notebooks/
│
├── tests/
│
├── requirements.txt
├── .env
├── README.md
└── main.py
```

## Installation

### Clone Repository

```bash
git clone https://github.com/your-username/crag.git
cd crag
```

### Create Virtual Environment

```bash
python -m venv venv
```

### Activate Environment

Windows:

```bash
venv\Scripts\activate
```

Linux/Mac:

```bash
source venv/bin/activate
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

## Environment Variables

Create a `.env` file:

```env
OPENAI_API_KEY=your_openai_api_key
TAVILY_API_KEY=your_tavily_api_key
```

## Workflow Components

### Retriever Node

Responsible for:

* Query embedding
* Similarity search
* Context retrieval

### Evaluator Node

Grades retrieved documents on:

* Relevance
* Completeness
* Confidence

Possible outcomes:

* Correct
* Incorrect
* Ambiguous

### Web Search Node

Triggered when retrieval quality is low.

Responsibilities:

* Search the web
* Collect supporting evidence
* Return relevant context

### Hybrid Retrieval Node

Combines:

* Internal knowledge base
* External web information

Useful for partially relevant retrievals.

### Generator Node

Uses:

* Retrieved documents
* Web results
* User query

To generate a grounded final answer.

## Example Workflow

### Input

```text
What are the latest developments in quantum computing?
```

### Scenario A

Internal documents contain relevant information.

```text
Retriever → Evaluator (Correct)
→ Answer Generated
```

### Scenario B

Internal documents are outdated.

```text
Retriever → Evaluator (Incorrect)
→ Tavily Search
→ Updated Answer
```

### Scenario C

Documents are partially relevant.

```text
Retriever → Evaluator (Ambiguous)
→ Hybrid Retrieval
→ Comprehensive Answer
```

## Benefits of CRAG

* Reduces hallucinations
* Improves answer reliability
* Handles incomplete retrievals
* Supports dynamic knowledge updates
* Better grounding than traditional RAG
* Production-ready architecture

## Future Enhancements

* Multi-agent retrieval strategy
* Query decomposition
* Self-reflection and answer verification
* Multi-vector retrieval
* Reranking with cross-encoders
* Long-term memory integration
* Human-in-the-loop review
* Observability with LangSmith

## References

* Retrieval-Augmented Generation (RAG)
* Corrective Retrieval-Augmented Generation (CRAG)
* LangGraph Documentation
* LangChain Documentation
* OpenAI API Documentation
* Tavily Search API

## License

MIT License

## Author

Pawan Kumar

M.Tech in Artificial Intelligence & Data Science

Passionate about Generative AI, Agentic AI, RAG Systems, LLM Engineering, and Applied Machine Learning.
