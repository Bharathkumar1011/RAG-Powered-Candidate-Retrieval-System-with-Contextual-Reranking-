# 🧠 Resume Knowledge Base Builder with Vector Search & Reranking

Resume Search and Retrieval System
This project implements a resume search and retrieval system using vector embeddings and reranking to efficiently match candidates to job criteria. It processes PDF resumes, converts them into a vector database, and retrieves relevant resumes using semantic search and reranking.
Table of Contents

Overview
Features
Requirements
Installation
Usage
Code Structure
How It Works
Example Query
Output
License

Overview
The system loads PDF resumes from a directory, optionally merges metadata from a CSV file, and processes them into a FAISS vector database using text splitting and embeddings from the thenlper/gte-small model. It supports complex queries to retrieve resumes based on job roles, skills, experience, and locations, with reranking to ensure high relevance.
Features

PDF Resume Loading: Extracts text from PDF resumes using PyPDFLoader.
Metadata Integration: Merges metadata from a CSV file (e.g., candidate ID, category).
Text Splitting: Chunks resume content using RecursiveCharacterTextSplitter for efficient processing.
Vector Database: Builds a FAISS vector store with embeddings for semantic search.
Semantic Search: Retrieves resumes using cosine similarity.
Reranking: Applies FlashrankRerank to refine results to the top 5 most relevant resumes.
Flexible Queries: Supports complex queries with AND, OR, NOT operators, and filters for experience and location.

Requirements

Python 3.8+
Required packages:torch
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



Installation

Clone the repository:
git clone <repository-url>
cd <repository-directory>


Install dependencies:
pip install -q torch transformers accelerate bitsandbytes langchain sentence-transformers faiss-cpu openpyxl datasets pypdf langchain-community langchain-huggingface ragatouille
pip install flashrank


Prepare your data:

Place PDF resumes in a directory (e.g., /path/to/resumes/data).
Optionally, provide a CSV file with metadata (e.g., /path/to/resumes/Resume.csv) including columns like ID.



Usage

Load and process resumes:
DATA_DIR = "/path/to/resumes/data"
CSV_PATH = "/path/to/resumes/Resume.csv"
docs = load_resumes(DATA_DIR, CSV_PATH)


Query the system:
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



Code Structure

Resume Loading: load_resumes function extracts text from PDFs and merges CSV metadata.
Text Splitting: Uses RecursiveCharacterTextSplitter with Markdown separators to chunk text.
Embedding Generation: Creates embeddings with thenlper/gte-small via HuggingFaceEmbeddings.
Vector Database: Stores embeddings in a FAISS vector store with cosine similarity.
Retrieval and Reranking: Retrieves top 30 resumes, then reranks to top 5 using FlashrankRerank.

How It Works

Resume Loading: Reads PDFs and merges metadata (if provided) based on resume IDs.
Text Processing: Converts resumes to LangchainDocument objects and splits into chunks (1000 characters, 100 overlap).
Embedding and Vector Store: Tokenizes chunks and generates embeddings, storing them in FAISS.
Retrieval and Reranking: Performs semantic search to retrieve top 30 resumes, then reranks to top 5.

Example Query
The example query searches for:

Roles: Machine Learning Engineer or Data Scientist.
Skills: Python (with Scikit-learn, TensorFlow, or PyTorch), SQL (with Spark, Hadoop, or ETL), cloud platforms, data visualization, statistical analysis or A/B testing, NLP/LLM/GenAI or recommendation systems.
Exclusions: No interns or students.
Experience: 3+ years.
Location: Remote, Mumbai, Bangalore, or Pune.

Output
A list of the top 5 resume IDs:
['resume_id_1', 'resume_id_2', 'resume_id_3', 'resume_id_4', 'resume_id_5']

License
MIT License
