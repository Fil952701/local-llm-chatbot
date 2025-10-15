# Sovereign RAG Chatbot

A production-oriented **Retrieval-Augmented Generation (RAG)** chatbot that runs **in-house**.  
It exposes a **JSON-only REST API** (FastAPI) so any PHP or web stack can consume it easily.

- **LLM**: pretrained, self-hosted (open-source or enterprise on-prem)
- **Embeddings**: multilingual (e.g., BAAI/bge-m3)
- **Reranker**: cross-encoder for high-precision retrieval
- **Output**: strict JSON schema with citations and safe fallbacks
- **Privacy**: no data leaves your perimeter

---

## Features

- 🔌 **HTTP API**: `POST /chat` → JSON; `POST /upsert` for ingestion; `GET /health`
- 🧠 **RAG Pipeline**: clean → chunk → embed → index (FAISS/Qdrant) → retrieve → rerank → generate
- 🧾 **Strict JSON**: schema validation, auto-repair, deterministic decoding
- 🛡️ **Safety**: “no-answer if out of context” + optional PII masking
- 🚀 **CPU/GPU Profiles**: CPU fallback (mini model) and GPU acceleration (7–8B)
- 🧪 **Evaluation Hooks**: golden set + RAG metrics (faithfulness, precision/recall)

---

## Tech Choices

- **LLM (select by profile)**
  - **GPU**: Qwen2.5-7B-Instruct or Llama-3.1-8B-Instruct  
  - **CPU fallback**: Phi-3.5-mini-instruct  
  - **Enterprise (no OSS)**: NVIDIA NIM / Cohere Private / IBM watsonx (same API contract)

- **Embeddings**: `BAAI/bge-m3` (multilingual)  
- **Reranker**: `BAAI/bge-reranker-v2-m3`  
- **Vector DB**: FAISS → Qdrant (HNSW)

---

## JSON Output Contract

```json
{
  "schema_version": "1.0",
  "answer": "string",
  "language": "it|en",
  "citations": [{"source": "string", "snippet": "string"}],
  "confidence": 0.0,
  "requested_action": "none|open_ticket|send_email",
  "metadata": {"latency_ms": 0, "model": "string"}
}
