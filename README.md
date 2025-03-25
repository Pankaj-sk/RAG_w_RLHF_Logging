# RAG-based Question Answering System

This is a **Retrieval-Augmented Generation (RAG)** application that combines document storage, vector search, and language model capabilities to provide accurate answers to user questions. The application is built with a focus on **maintainability, logging, metrics collection, and versioning** - all important components of **ML/LLM Operations (MLOps/LLMOps).**

## Architecture Overview

The application has a clear separation of concerns with multiple components:

### 1. Document Management System
- Python scripts for adding and updating documents in a **vector database (ChromaDB)**
- Support for **versioning document collections**

### 2. API Layer
- **Express.js** server providing REST endpoints for document retrieval, search, and question answering
- Middleware for **error handling and logging**

### 3. Front-end Interface
- Simple **web UI** for interacting with the application
- Components for **document listing, search, and chat**

### 4. Observability Infrastructure
- **Comprehensive logging system**
- **Metrics collection** for performance monitoring
- **Version tracking** for data and models

## Key Components

### 1. Vector Database Management (ChromaDB)
The `update_db.py` script is responsible for:
- Reading and processing text documents
- Splitting documents into chunks for better retrieval
- Calculating embeddings using **OpenAI's embedding model**
- Storing document chunks with their embeddings in **ChromaDB**
- Supporting **document updates and versioning**

**Key design features:**
- Documents are uniquely identified using **content hashes**
- Chunks maintain **metadata** about their source document
- **Versioning** allows tracking database updates
- **Robust error handling and logging** throughout

### 2. Prompt Management
The `upload.py` script provides:
- A way to store and manage prompts in **ChromaDB**
- **Version tracking** for prompts
- Unique identification of prompts through **content hashing**

**This enables:**
- Consistency in how the system prompts the LLM
- Ability to track **prompt changes over time**
- Potentially **A/B testing different prompts**

### 3. Node.js Backend
The server consists of:
- A **main application entry point** (`index.js` and `app.js`)
- **API routes** for various functions
- **Models** for interacting with ChromaDB and OpenAI
- **Utility functions** for logging and metrics

**Key Endpoints:**
- `GET /api/documents` - Lists all documents
- `POST /api/search` - Searches for documents using **semantic search**
- `POST /api/ask` - Answers questions using **RAG**

### 4. RAG Implementation
The core **retrieval-augmented generation** is implemented in `models/retrieval.js`:
- When a **question** is received, it calculates the **embedding** using OpenAI
- It retrieves the **most relevant document chunks** from ChromaDB
- It constructs a **prompt** that includes the retrieved **context and the question**
- It sends this to **OpenAI's chat model** to generate an answer
- The answer is returned to the user

### 5. Logging and Metrics
The application implements comprehensive observability:
- **Winston logger** for structured, multi-level logging
- **Metrics collection** for API endpoints, including:
  - **Response times**
  - **Request counts**
  - **Error rates**
  - **Query characteristics**
- Metrics are **buffered in memory** and periodically **flushed to disk**
- **Process hooks** ensure metrics are saved even on application shutdown

### 6. Testing
The application includes **jest-based tests**:
- **API endpoint tests**
- **Model function tests**
- **Mock implementations** for external dependencies (ChromaDB, OpenAI)

## Design Choices and Patterns

### 1. Modularity
- The application follows a **modular design** with clear separation of concerns:
  - **Data storage and retrieval**
  - **Business logic**
  - **API endpoints**
  - **User interface**
  - **Logging and metrics**
- This makes the code **more maintainable** and **easier to update**.

### 2. Asynchronous Processing
- The application makes extensive use of **async/await**:
  - For **API calls** to ChromaDB and OpenAI
  - For **file operations**
  - For **database queries**
- This ensures the application remains **responsive** even when performing I/O-bound operations.

### 3. Error Handling
- Comprehensive **error handling** is implemented throughout:
  - **Try/catch** blocks around all async operations
  - **Detailed error logging**
  - **Error metrics tracking**
  - **Graceful error responses** to the client

### 4. Environment Configuration
- The application uses **environment variables** for configuration:
  - **OpenAI API key**
  - **ChromaDB URL**
  - **Port settings**
  - **Environment name (development, production)**
- This follows the **twelve-factor app methodology** for configuration.

### 5. Versioning
- The application implements **versioning** for:
  - **Document collections**
  - **Prompts**
  - **Embeddings**
- This allows for **tracking changes** and potentially **rolling back** to previous versions.

### 6. REST API Design
- The API follows **RESTful principles**:
  - **Resource-based URLs**
  - **Appropriate HTTP methods** (GET, POST)
  - **JSON request/response format**
  - **Status codes** for success/error handling

## Technologies Used

### Backend
- **Node.js** with **Express.js**
- **Python** for data processing

### Databases
- **ChromaDB** for **vector storage**
- **File-based storage** for metrics and logs

### AI Services
- **OpenAI** for **embeddings and text generation**

### Tooling
- **Winston** for logging
- **Jest** for testing
- **ESLint** for code quality

## Deployment Considerations
The application is designed for **easy deployment**:
- **Docker-ready structure**
- **Environment variable configuration**
- **Clear separation of frontend and backend**
- **Storage directories** for persistent data

## Setup and Installation

### 1. Clone the repository:
```bash
git clone <repository_url>
cd <repository_folder>
```

### 2. Install Node.js dependencies:
```bash
npm install
```

### 3. Install Python dependencies (if required):
```bash
pip install -r requirements.txt
```

### 4. Set up environment variables in a `.env` file:
```
OPENAI_API_KEY=<your_openai_api_key>
CHROMA_DB_URL=<your_chromadb_url>
PORT=3000
NODE_ENV=development
```

### 5. Run ChromaDB:
```bash
chroma run --path ./data
```

### 6. Run the application:
```bash
npm start
```

---

This README provides a detailed **overview, architecture, setup, and deployment guide** for your **RAG-based Question Answering System**. 🚀

