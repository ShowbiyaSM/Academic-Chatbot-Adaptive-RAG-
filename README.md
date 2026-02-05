# Academic Chatbot

### Intelligent Q&A System for Academic PDFs

[![Python 3.9+](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://python.org)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT-green.svg)](https://openai.com)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org)

A smart chatbot that answers questions from uploaded academic PDFs using adaptive retrieval and validation techniques.

---

## ✨ What It Does

* **Upload academic PDFs** - research papers, textbooks, articles
* **Ask natural language questions** - get accurate answers with source validation
* **Handles vague queries** - expands questions using document context
* **Prevents wrong answers** - checks for hallucinations and correctness
* **Fast responses** - remembers similar questions using semantic caching

## 🚀 Quick Start

1. **Clone the repository**

```bash
git clone https://github.com/ShowbiyaSM/Academic-Chatbot-Adaptive-RAG-.git
cd academic-chatbot
```


2. **Install dependencies**

```bash
pip install -r requirements.txt
```



3. **Set up OpenAI API key**

Copy the template

```bash
cp .env.example .env
```

Edit .env and add your API key

```env
OPENAI_API_KEY=your_key_here
```



4. **Run the notebook**

```bash
jupyter notebook Academic_Chatbot.ipynb
```



## 🛠️ How It Works


### Step-by-Step Process

1. **PDF Upload & Processing**
   - Upload academic PDFs through the interface
   - Extract text using `pdfplumber`
   - Split into semantic chunks with `langchain-text-splitters`

2. **Vector Storage**
   - Convert chunks to embeddings using `sentence-transformers/all-MiniLM-L6-v2`
   - Store embeddings in FAISS vector database for fast retrieval

3. **Query Processing**
   - User asks a natural language question
   - Convert query to embedding
   - Check semantic cache for similar previous queries

4. **Adaptive Retrieval**
   - FAISS similarity search finds relevant chunks
   - **Adaptive Step**: Check if query is related to PDF content
   - **Adaptive Step**: Expand vague queries using chunk keywords
   - **Adaptive Step**: Validate if chunks are sufficient to answer

5. **Answer Generation**
   - Send query + top chunks to OpenAI GPT
   - Generate answer strictly based on provided context
   - **Key**: GPT used only here (cost optimization)

6. **Answer Validation**
   - Check for hallucinations using NLI model (`facebook/bart-large-mnli`)
   - Validate correctness with heuristic checks
   - **Adaptive Step**: Regenerate if validation fails

7. **Response & Learning**
   - Display final answer to user
   - Store in semantic cache for future similar queries
   - Log interactions for continuous improvement

## 📁 Project Structure

```
academic-chatbot/
├── Academic_Chatbot.ipynb # Complete implementation
├── requirements.txt # Python dependencies
├── .env.example # API key setup template
└── README.md # This file
```

## 🎯 Key Features

* **Adaptive Retrieval** - Adjusts search based on query type and similarity
* **Query Expansion** - Improves vague questions using document keywords
* **Answer Validation** - NLI model checks hallucination + correctness
* **Semantic Caching** - FAISS-based cache for similar questions
* **Cost Optimized** - Open-source models for validation, GPT only for answers

## 💡 Learning Project

This project implements:

* **RAG (Retrieval-Augmented Generation)** pipeline
* **Semantic search** with FAISS and Sentence Transformers
* **Adaptive routing** based on query-document similarity
* **Answer validation** using NLI models
* **Production optimizations** like caching and early exits

Built while learning about NLP systems and RAG architectures.

---

*Perfect for students, researchers, and anyone working with academic documents.*
