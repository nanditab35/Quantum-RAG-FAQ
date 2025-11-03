# Quantum Mechanics Vector RAG with Reranking
- This repository contains the implementation of a basic Vector-based Retrieval-Augmented Generation (RAG) pipeline designed to answer questions based on a corpus of Quantum Mechanics (QM) FAQs.
- The key feature of this project is the integration of a cross-encoder reranker to improve the relevance of retrieved documents before they are passed to the final Language Model (LLM).

🚀 **Key Features**:
- Vector Database: Utilizes a standard vector store (FAISS) to index the high-dimensional embeddings of the QM text corpus.
- Contextual Reranking: Implements a specialized cross-encoder (e.g., jina-reranker-v2) to re-score the top-$K$ documents retrieved from the vector store. This significantly boosts precision by focusing on true contextual relevance rather than just vector proximity.
- Question Answering: Generates grounded, factual answers using the highly relevant, reranked context.
- Domain: Focused on complex, technical Q&A within the field of Quantum Mechanics.

🛠 **Architecture Components**:
| Component | Role | Example Technology/Model |
|---|---|---|
| Embedding Model | Converts text into dense vector representations. | all-MiniLM-L6-v2 |
| Vector Store | Stores and performs initial nearest-neighbor search. | FAISS |
| Retriever | Fetches the top $K$ document candidates based on similarity. | Custom function based on Vector Store API |
| Reranker | Rescores candidates based on semantic cross-attention score. | jinaai/jina-reranker-v2-base-multilingual |
| Generator (LLM) | Synthesizes the final answer from the reranked context. | GPT-3.5 |

💡 **Usage and Execution**:
- Environment Setup: Install required libraries: !pip install transformers sentence-transformers numpy faiss-cpu
- Data Ingestion: Load the QM text corpus, split it into chunks, and generate embeddings for storage in the Vector Store.
- RAG Query: Execute the pipeline using the query function:
    - User query is embedded.
    - Top $K$ documents are retrieved.
    - Reranker scores the $K$ documents and selects the final top $N$.
    - LLM generates the answer using the top $N$ context pieces.
 
⚠️ **Known Issues**:
- Reranker Cost Anomaly: During testing, the jinaai/jina-reranker-v2-base-multilingual configuration was observed to cause disproportionate API call volume to the generator LLM endpoint, necessitating caution and cost profiling if re-enabled.
  

LLM generates the answer using the top $N$ context pieces.
