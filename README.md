# Production-Ready RAG AI Agent in Python

This project demonstrates how to build and deploy a production-ready Retrieval Augmented Generation (RAG) AI agent using Python, incorporating essential features like observability, logging, retries, throttling, and rate limiting. The application allows users to upload PDF documents and ask questions, with the AI agent providing answers based on the content of those documents.

-----

## Features

  * **PDF Ingestion:** Upload and process PDF documents, breaking them into manageable chunks.
  * **Vector Database Integration:** Store document embeddings in a local Qdrant vector database for efficient similarity search.
  * **Intelligent Querying:** Ask natural language questions about the uploaded PDFs, and receive concise answers augmented by the document content.
  * **Production-Grade Orchestration:** Leverage Ingest for:
      * **Observability & Logging:** Gain deep insights into every step of the AI application's workflow.
      * **Automatic Retries:** Handle transient errors gracefully with configurable retry mechanisms.
      * **Throttling & Rate Limiting:** Control the flow of requests to prevent abuse and manage resource usage.
      * **Flow Control:** Manage asynchronous operations and parallel execution of steps.
  * **Streamlit Frontend:** An intuitive web interface for uploading PDFs and interacting with the RAG agent.


##  Technologies Used

  * **Python:** The core programming language.
  * **FastAPI:** For building the backend API endpoints.
  * **Streamlit:** For creating the interactive web user interface.
  * **Ingest:** An open-source orchestration tool for managing workflows, observability, and reliability in AI applications.
  * **Llama Index:** Used for reading, parsing, and chunking PDF documents.
  * **Qdrant:** A high-performance vector database, runnable locally via Docker, for storing and searching vector embeddings.
  * **OpenAI:** Provides embedding models (e.g., `text-embedding-3-large`) and large language models (e.g., `GPT-4o-mini`) for text embedding and answer generation.
  * **Docker:** For running the Qdrant vector database locally.
  * **UV:** A fast Python package installer and dependency management tool.


##  Setup and Installation

Follow these steps to set up and run the project locally.

### Prerequisites

  * Python 3.8+
  * Node.js (for Ingest CLI)
  * Docker Desktop (for Qdrant vector database)

### 1\. Clone the Repository

```bash
git clone <repository_url>
cd <project_directory>
```

### 2\. Install Python Dependencies

The project uses `uv` for dependency management.

```bash
uv init
uv add fastapi ingest llama-index-core llama-index-readers-file python-dotenv qdrant-client uvicorn streamlit openai
```

### 3\. Configure OpenAI API Key

Create a `.env` file in the root directory of your project and add your OpenAI API key:

```ini
OPENAI_API_KEY="your_openai_api_key_here"
```

You can obtain an API key from [platform.openai.com/api-keys](https://platform.openai.com/api-keys).

### 4\. Run Qdrant Vector Database with Docker

First, create a directory for Qdrant storage:

```bash
mkdir qdrant_storage
```

Then, run the Qdrant Docker container.

```powershell
docker run -d --name qdrant_rag_db -p 6333:6333 -v "$(pwd)/qdrant_storage:/qdrant/storage" qdrant/qdrant
```

Verify that Qdrant is running by checking Docker Desktop or running `docker ps`.

### 5\. Start the FastAPI Application

In a new terminal, navigate to your project directory and run the FastAPI server:

```bash
uvicorn main:app --reload
```

The API will be running on `http://127.0.0.1:8000`.

### 6\. Run the Ingest Development Server

In another new terminal, start the Ingest local development server. This server orchestrates your AI functions and provides observability.

```bash
npx @ingest-dev/cli@latest dev -u http://127.0.0.1:8000/api/ingest --no-discovery
```

The Ingest UI will be accessible at `http://localhost:8288`. Open this URL in your browser to monitor your application's runs, logs, and functions.

### 7\. Launch the Streamlit Frontend

Finally, in a separate terminal, start the Streamlit application:

```bash
streamlit run streamlit_app.py
```

This will open the Streamlit UI in your web browser, where you can upload PDFs and interact with your RAG agent.

-----

## Usage

1.  **Upload a PDF:** In the Streamlit frontend, use the upload functionality to select and process a PDF document. The Ingest server will track the ingestion process.
2.  **Ask Questions:** Once a PDF is ingested, you can type questions related to its content into the text input field.
3.  **Get Answers:** The AI agent will search the vectorized documents and provide answers based on the most relevant context found.
4.  **Monitor with Ingest:** Observe the execution of your `rag_ingest_PDF` and `rag_query_pdf_ai` functions in the Ingest development server UI (`http://localhost:8288`). You can see detailed logs, retry failed steps, and analyze performance.

----
