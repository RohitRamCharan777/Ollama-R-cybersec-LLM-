# AI-Powered Cybersecurity RAG Chatbot

An AI-powered cybersecurity RAG chatbot designed to retrieve vulnerability information from a local offline knowledge base (`cybersecurity_facts.txt`) and leverage local Retrieval-Augmented Generation (RAG) to generate context-aware summaries, technical impact analyses, and actionable remediation recommendations.

The application runs fully locally utilizing:
- **Streamlit** for a premium, cybersecurity-themed interactive chatbot dashboard.
- **Ollama** for running a local LLM (`llama3.2`) and embedding model (`nomic-embed-text`).
- **In-Memory Cosine Similarity** for zero-dependency vector similarity search.
- **`cybersecurity_facts.txt`** for storing raw security intelligence facts.

---

## System Architecture

```
                      +-------------------+
                      |   Web UI (Streamlit) |
                      +---------+---------+
                                |
                    RAG Local   | 
                    Retrieval   v
                      +-------------------+
                      | Local facts file  |
                      | & In-Memory DB    |
                      +---------+---------+
                                |
                    Ollama API  | (Port 11434)
                                v
                      +-------------------+
                      | Local Ollama LLM  |
                      | & Embeddings      |
                      +-------------------+
```

---

## Step-by-Step Installation Guide

### Prerequisites
- **Python**: Ensure you have Python 3.10+ installed on your system.
- **uv**: The project uses `uv` as the package manager. If you do not have it, install it using:
  ```powershell
  # Windows Powershell
  powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
  ```

---

### Step 1: Install and Configure Ollama
1. Download Ollama for your operating system: [https://ollama.com/download](https://ollama.com/download)
2. Run the installer and launch the Ollama application.
3. Open a terminal and pull the LLM and Embedding models:
   ```bash
   # Pull the lightweight LLM (3 Billion parameters, fast & optimized for local execution)
   ollama pull llama3.2

   # Pull the high-performance text embedding model
   ollama pull nomic-embed-text
   ```

---

### Step 2: Install Project Dependencies
Use `uv` to automatically synchronize and install the Python virtual environment and its dependencies:
```bash
# Install dependencies in a local virtual environment (.venv)
uv sync
```

---

### Step 3: Run the Application

Start the Streamlit chatbot app directly:
```bash
uv run streamlit run app.py
```
- The frontend chat dashboard will start and automatically open in your default browser at [http://localhost:8501](http://localhost:8501).

---

## How to Use the Chatbot

1. **Ask Security Queries**: Enter questions like *"Explain what SQL Injection is"* or *"What is CVE-2021-44228?"*.
2. **Context Sidebar**: As you chat, the app automatically finds the top-3 matching chunks from your `cybersecurity_facts.txt` file and displays them in the sidebar with their cosine similarity score.
3. **Dynamic Configurations**: Choose different LLM and embedding models in the sidebar from any model currently pulled in your local Ollama instance.
4. **Conversational Memory**: Ask follow-up questions (e.g. *"What is the patch configuration for it?"*) and the assistant will reply in context of the chat history.
