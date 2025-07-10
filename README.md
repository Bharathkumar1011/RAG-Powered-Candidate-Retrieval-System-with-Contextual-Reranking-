# 🧠 Resume Knowledge Base Builder with Vector Search & Reranking

This project implements a resume search and retrieval system using vector embeddings and reranking to efficiently match candidates to job criteria. It processes PDF resumes, converts them into a vector database, and retrieves relevant resumes using semantic search and reranking.

## Table of Contents

-   [Overview]
-   [Features]
-   [Requirements]
-   [Installation]
-   [Usage]
-   [Code Structure]
-   [How It Works]
-   [Example Query]
-   [Output]
-   [License]

# Resume Search and Retrieval System

This project implements a resume search and retrieval system using vector embeddings and reranking to efficiently match candidates to job criteria. It is designed to handle large datasets of resumes across diverse fields, processing PDF resumes into a vector database for semantic search and reranking.

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Code Structure](#code-structure)
- [How It Works](#how-it-works)
- [Example Query](#example-query)
- [Output](#output)
- [License](#license)

## Overview
The system is built to process large datasets of resumes from various professional fields, such as engineering, data science, finance, and more. It loads PDF resumes from a directory, optionally merges metadata from a CSV file, and transforms them into a FAISS vector database using text splitting and embeddings from the `thenlper/gte-small` model. The system supports complex queries to retrieve resumes based on job roles, skills, experience, and locations, with reranking to ensure high relevance, making it suitable for scalable recruitment pipelines.

## Features
- **Large-Scale PDF Resume Loading**: Efficiently extracts text from large collections of PDF resumes across diverse fields using `PyPDFLoader`.
- **Metadata Integration**: Merges metadata from a CSV file (e.g., candidate ID, category) to enrich resume data.
- **Text Splitting**: Chunks resume content using `RecursiveCharacterTextSplitter` for efficient processing of large datasets.
- **Vector Database**: Builds a FAISS vector store with embeddings for semantic search, optimized for handling extensive resume collections.
- **Semantic Search**: Retrieves resumes using cosine similarity, scalable for large datasets.
- **Reranking**: Applies `FlashrankRerank` to refine results to the top 5 most relevant resumes, improving precision for targeted searches.
- **Flexible Queries**: Supports complex queries with AND, OR, NOT operators, and filters for experience and location, adaptable to diverse job requirements.

## Requirements

-   Python 3.8+
-   Required packages:
    
    ```bash
    torch
    transformers
    accelerate
    bitsandbytes
    langchain
    sentence-transformers
    faiss-cpu
    openpyxl
    datasets
    pypdf
    langchain-community
    langchain-huggingface
    ragatouille
    flashrank
    
    ```
    

## Installation

1.  Clone the repository:
    
    ```bash
    git clone https://github.com/Bharathkumar1011/RAG-Powered-Candidate-Retrieval-System-with-Contextual-Reranking-/tree/main.git
    cd RAG-Powered-Candidate-Retrieval-System-with-Contextual-Reranking 
    ```
    
2.  Install dependencies:
    
    ```bash
    pip install -q torch transformers accelerate bitsandbytes langchain sentence-transformers faiss-cpu openpyxl datasets pypdf langchain-community langchain-huggingface ragatouille
    pip install flashrank
    
    ```
    
3.  Prepare your data:
    
    -   Place PDF resumes in a directory (e.g., `/path/to/resumes/data`).
    -   Optionally, provide a CSV file with metadata (e.g., `/path/to/resumes/Resume.csv`) including columns like `ID`.

## Usage

1.  Load and process resumes:
    
    ```python
    DATA_DIR = "/path/to/resumes/data"
    CSV_PATH = "/path/to/resumes/Resume.csv"
    docs = load_resumes(DATA_DIR, CSV_PATH)
    
    ```
    
2.  Query the system:
    
    ```python
    query = """
    "Machine Learning Engineer" OR "Data Scientist" AND (
      ("Python" AND ("Scikit-learn" OR "TensorFlow" OR "PyTorch"))
      ("SQL" AND ("Spark" OR "Hadoop" OR "ETL"))
      ("AWS" OR "GCP" OR "Azure" OR "MLOps")
      ("Tableau" OR "Power BI" OR "data visualization")
      ("statistical analysis" OR "A/B testing")
      ("NLP" OR "LLM" OR "GenAI" OR "recommendation systems")
    )
    NOT ("intern" OR "student")
    Years: "3+ years"
    Location: "Remote" OR "Mumbai" OR "Bangalore" OR "Pune"
    """
    compressed_docs = compression_retriever.invoke(query)
    print([doc.metadata.get("id") for doc in compressed_docs])
    
    ```
    

## Code Structure

-   **Resume Loading**: `load_resumes` function extracts text from PDFs and merges CSV metadata.
-   **Text Splitting**: Uses `RecursiveCharacterTextSplitter` with Markdown separators to chunk text.
-   **Embedding Generation**: Creates embeddings with `thenlper/gte-small` via `HuggingFaceEmbeddings`.
-   **Vector Database**: Stores embeddings in a FAISS vector store with cosine similarity.
-   **Retrieval and Reranking**: Retrieves top 30 resumes, then reranks to top 5 using `FlashrankRerank`.

## How It Works

1.  **Resume Loading**: Reads PDFs and merges metadata (if provided) based on resume IDs.
2.  **Text Processing**: Converts resumes to `LangchainDocument` objects and splits into chunks (1000 characters, 100 overlap).
3.  **Embedding and Vector Store**: Tokenizes chunks and generates embeddings, storing them in FAISS.
4.  **Retrieval and Reranking**: Performs semantic search to retrieve top 30 resumes, then reranks to top 5.

## Example Query

The example query searches for:

-   Roles: Machine Learning Engineer or Data Scientist.
-   Skills: Python (with Scikit-learn, TensorFlow, or PyTorch), SQL (with Spark, Hadoop, or ETL), cloud platforms, data visualization, statistical analysis or A/B testing, NLP/LLM/GenAI or recommendation systems.
-   Exclusions: No interns or students.
-   Experience: 3+ years.
-   Location: Remote, Mumbai, Bangalore, or Pune.

## Output

A list of the top 5 resume IDs:

```python
['resume_id_1', 'resume_id_2', 'resume_id_3', 'resume_id_4', 'resume_id_5']

```

## License

MIT License

