# Basic PDF RAG Assistant

A beginner-level Retrieval-Augmented Generation (RAG) application built as a hands-on exercise during my Data Science and AI bootcamp.

The application lets a user upload a PDF, ask questions about its content, and receive answers generated using relevant passages retrieved from that document. It demonstrates the basic RAG workflow: document ingestion, text chunking, embedding generation, vector similarity search, context construction, and LLM-based response generation.

## What it does

1. Accepts a PDF upload through a Streamlit interface.
2. Extracts text from the first three pages of the uploaded PDF using `pypdf`.
3. Splits the extracted text into chunks of 800 characters.
4. Generates normalized embeddings for each chunk using the `all-MiniLM-L6-v2` Sentence Transformers model.
5. Stores the embeddings in a FAISS `IndexFlatIP` index for similarity search.
6. Converts the user's question into an embedding and retrieves the three most relevant chunks.
7. Adds the retrieved text to a prompt and sends it to Google's Gemini API to generate an answer.
8. Displays the generated answer and the retrieved chunks so the user can inspect the supporting context.

The prompt instructs the model to answer only from the supplied document context and to say when the answer is not available in that context.

## RAG workflow

```text
PDF upload
   |
   v
Extract text (first 3 pages)
   |
   v
Split text into chunks
   |
   v
Generate embeddings
   |
   v
Build FAISS similarity-search index
   |
   v
User question -> question embedding
   |
   v
Retrieve top 3 relevant chunks
   |
   v
Build context-augmented prompt
   |
   v
Gemini generates an answer
   |
   v
Display answer and retrieved chunks
```

## Technologies used

- Python
- Streamlit
- PyPDF
- Sentence Transformers (`all-MiniLM-L6-v2`)
- FAISS
- NumPy
- Google Gemini API (`google-genai`)

## Run locally

### 1. Clone the repository

```bash
git clone https://github.com/SharveshV/MyfirstRAGapp.git
cd MyfirstRAGapp
```

### 2. Create and activate a virtual environment (recommended)

```bash
python -m venv .venv
```

macOS / Linux:

```bash
source .venv/bin/activate
```

Windows:

```bash
.venv\\Scripts\\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

The application uses these packages:

```text
streamlit
faiss-cpu
numpy
pypdf
sentence-transformers
google-genai
```

If the repository's `requirements.txt` does not yet list all of these packages, install them with:

```bash
pip install streamlit faiss-cpu numpy pypdf sentence-transformers google-genai
```

### 4. Start the application

```bash
streamlit run app.py
```

Enter a Google Gemini API key in the sidebar when prompted, upload a text-based PDF, select **Process PDF**, and then ask a question about the document.

## Scope and current limitations

This is a basic learning project intended to demonstrate the core RAG pipeline, not a production-ready document-question-answering system.

- Only the first three pages of a PDF are processed.
- Text is split into fixed-size chunks of 800 characters, without a configurable overlap.
- Retrieval uses a local FAISS index and returns the top three chunks.
- The index and chunks are held in the Streamlit session; there is no persistent vector database.
- The application relies on extractable PDF text and does not include OCR for scanned documents.
- Answer quality depends on the extracted text, retrieved context, and Gemini response.

## Learning objective

Built during my bootcamp as an introductory, hands-on implementation to understand how embeddings, vector similarity search, retrieval, prompt context, and LLM generation fit together in a basic RAG application.
