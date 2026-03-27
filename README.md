# SentinelRAG-TinyLlama

A high-performance, lightweight Retrieval-Augmented Generation (RAG) system built with **TinyLlama-1.1B** and **FAISS**. This project demonstrates efficient document retrieval and grounded answer generation, reducing hallucinations in Small Language Models (SLMs) using advanced architectural filtering.

## 🚀 Key Highlights (for CV)
- **RAG Pipeline for TinyLlama:** Integrated FAISS vector search with TinyLlama-1.1B for grounded Q&A on the SQuAD dataset.
- **High Faithfulness:** Achieved strict context adherence using Few-Shot prompting and a Similarity Threshold of 0.2.
- **Architectural Hallucination Filter:** Implemented a relevancy shield using FAISS similarity scores (Euclidean/IP) to prune ungrounded queries (e.g., correctly rejected a generic PM question with a low score of 0.07).
- **Optimized Retrieval:** Implemented `all-MiniLM-L6-v2` embeddings for sub-millisecond latency.

## 🛠️ Tech Stack
- **Model:** TinyLlama-1.1B-Chat-v1.0
- **Retriever:** FAISS (Facebook AI Similarity Search)
- **Embeddings:** HuggingFace `sentence-transformers`
- **Dataset:** SQuAD (Stanford Question Answering Dataset)

## 📖 How it Works
1. **Indexing:** Unique contexts from the SQuAD dataset are embedded using a MiniLM model and stored in a FAISS index.
2. **Retrieval & Filtering:** User queries are converted to vectors and matched against the index. If the top similarity score is below **0.2**, the system preemptively returns "NOT FOUND" to prevent hallucinations.
3. **Generation:** For relevant matches, the context is combined with the query in a Few-Shot prompt and fed into TinyLlama.

## 💻 Setup & Run
1. Clone the repo:
   ```bash
   git clone [your-repo-link]
   cd IAAIR
   ```
2. Install dependencies:
   ```bash
   pip install torch transformers sentence-transformers faiss-cpu datasets
   ```
3. Run the pipeline:
   ```bash
   python rag_pipeline.py
   ```

## 📊 Benchmark Results
| Query Type | Relevancy Score | Output | Status |
| :--- | :--- | :--- | :--- |
| **In-Context (Stadium)** | 0.28 | "The stadium has a capacity of 80,000." | ✅ Successful |
| **In-Context (Population)** | >0.4 | "12,179 students..." | ✅ Successful |
| **Out-of-Context (Trivia)** | 0.07 | "NOT FOUND" | 🛡️ Correctly Filtered |
