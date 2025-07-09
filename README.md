# 🧠 Resume Knowledge Base Builder with Vector Search & Reranking

This project builds a structured, searchable vector database from resume PDFs. It supports semantic retrieval using transformer embeddings and re-ranking using Flashrank, making it useful for resume filtering, candidate search, and intelligent job matching.

---

## 🚀 Features

- 📄 PDF Resume Parsing & Metadata Enrichment
- 🧹 Text Chunking for Embedding
- 🤖 Embedding with Sentence Transformers (`gte-small`)
- 📦 Vector Database with FAISS
- 🔍 Advanced Search Query Support
- ⚡ Flashrank-based Reranking of Results
- 🧠 Contextual Compression using LangChain

---

## 📦 Installation

Install all required Python packages using pip:

```bash
pip install -q torch transformers accelerate bitsandbytes langchain sentence-transformers faiss-cpu openpyxl datasets pypdf langchain-community langchain-huggingface ragatouille
pip install flashrank
