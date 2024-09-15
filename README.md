

PDF Document Retrieval System with LangChain

This project implements a Document Retrieval System using LangChain and Hugging Face's Transformer models. The system is designed to load and split PDF documents, convert them into vector embeddings, and efficiently retrieve context based on user queries. A Gradio interface is provided for interactive Q&A functionality.

 Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Installation](#installation)
- [Project Structure](#project-structure)
- [How to Run](#how-to-run)
- [Usage](#usage)
- [Gradio Interface](#gradio-interface)
- [Caching](#caching)
- [Background Tasks](#background-tasks)
- [Rate Limiting](#rate-limiting)
- [To-Do](#to-do)
- [Contributing](#contributing)
- [License](#license)

 Overview

This project builds a **PDF Document Retrieval System** for answering user queries based on stored documents. The system utilizes **LangChain**, **Hugging Face's sentence-transformers**, and **FAISS** for vector-based document retrieval. Users can ask questions, and the system will return relevant context from the PDF documents, thanks to a **Gradio** interface.

The key components of this system include:
- Loading and splitting PDF documents into manageable chunks.
- Converting these chunks into vector embeddings using `sentence-transformers/all-MiniLM-L6-v2`.
- Storing and retrieving document embeddings via **FAISS** for fast similarity search.
- Leveraging Hugging Face's large language models to generate answers.
- A user-friendly **Gradio** interface for interacting with the system.

 Features

- Document Upload & Processing**: Upload PDF files from Google Drive, split them into text chunks, and convert them to embeddings.
- Efficient Retrieval**: Uses FAISS for efficient similarity search among document embeddings.
- Q&A with LLM**: Supports interactive question answering using a Hugging Face model.
- Gradio Interface**: A web-based interface that allows users to input queries and receive answers.
- Caching**: Improves performance by caching document retrieval responses.
- Rate Limiting**: Limits the number of queries per user to prevent overloading the system.
- Background Scraping (Optional)**: Scrapes news articles in the background.

 Tech Stack

- LangChain**: Framework for building language model applications.
- Hugging Face Hub**: Provides pre-trained models for embeddings and LLMs.
- FAISS**: Efficient similarity search and clustering of dense vectors.
- Gradio**: Easy-to-use web interface for ML models.
- Python Libraries**: `sentence-transformers`, `torch`, `pandas`, `sqlite3`, `fastapi`.
- Google Colab**: For cloud-based development.

 Installation

To set up the project locally:

1. Clone the repository:
   ```bash
   git clone https://github.com/<your-username>/<your-repo-name>.git
   ```

2. Install dependencies using pip:
   ```bash
   pip install -r requirements.txt
   ```

3. Alternatively, if you're using Google Colab, ensure all dependencies are installed:
   ```bash
   !pip -q install langchain torch faiss-cpu huggingface-hub accelerate sentence_transformers gradio==3.48.0
   ```

4. Set your Hugging Face API Token:
   ```python
   from getpass import getpass
   HUGGINGFACEHUB_API_TOKEN = getpass("Enter your Hugging Face API Token:")
   ```

 Project Structure

```bash
app/
├── main.py                # Main FastAPI or Gradio app
├── retriever.py           # Handles document retrieval logic
├── cache.py               # Caching system implementation
├── rate_limit.py          # Rate limiting logic
├── scraper.py             # Optional: Background news scraper
├── requirements.txt       # Python dependencies
├── Dockerfile             # Docker configuration file
└── README.md              # This readme file
```

 How to Run

1. Mount Google Drive to load your PDFs:
   ```python
   from google.colab import drive
   drive.mount('/content/drive')
   ```

2. Load documents, split them, and generate embeddings:
   ```python
   loader = PyPDFDirectoryLoader("/content/drive/MyDrive/RAG")
   docs = loader.load()
   text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=50)
   text_chunks = text_splitter.split_documents(docs)
   embeddings = HuggingFaceEmbeddings(model_name="sentence-transformers/all-MiniLM-L6-v2")
   vector_store = FAISS.from_documents(text_chunks, embedding=embeddings)
   ```

3. Start the Gradio interface:
   ```python
   iface.launch()
   ```



To-Do

- [ ] Add support for CSV document loading.
- [ ] Enhance error handling in the retriever.
- [ ] Integrate with other LLMs like OpenAI GPT-4.
- [ ] Add more extensive tests for edge cases.
- [ ] Deploy the system on AWS or any cloud provider using Docker.

