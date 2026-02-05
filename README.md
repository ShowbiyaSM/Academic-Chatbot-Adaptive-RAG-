[![Python 3.9+](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://python.org)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT-green.svg)](https://openai.com)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org)


## 🔄 Application Flow

This project follows a **Retrieval-Augmented Generation (RAG)** pipeline to answer questions based on uploaded academic PDFs.

### Step-by-step Flow

1. **PDF Upload**
   - Users upload one or more PDF files via the Streamlit UI.

2. **Text Extraction**
   - PDF text is extracted page by page using `pdfplumber`.

3. **Text Chunking**
   - Extracted text is split into overlapping chunks using `RecursiveCharacterTextSplitter`.
   - Chunking improves retrieval accuracy and context relevance.

4. **Embedding Generation**
   - Each text chunk is converted into a vector embedding using  
     `sentence-transformers/all-MiniLM-L6-v2`.

5. **Vector Storage**
   - Embeddings are stored in a FAISS vector database along with chunk metadata.

6. **Question Input**
   - The user enters a natural language question.

7. **Query Embedding**
   - The question is converted into an embedding using the same embedding model.

8. **Relevant Chunk Retrieval**
   - FAISS performs similarity search to retrieve the top-k most relevant chunks.

9. **Answer Generation**
   - Retrieved chunks are passed as context to the LLM (`gpt-4.1-nano`) via a Navigate-compatible OpenAI API.
   - The model generates an answer strictly based on the provided content.

10. **Response Display**
    - The final answer is displayed in the Streamlit interface.

### 🔑 Key Concept
> The chatbot does **not hallucinate** — it answers **only** using the retrieved PDF content.


