# File AI Agent — Chat with your PDFs locally, for free

> A fully local, open-source RAG (Retrieval Augmented Generation) app that lets you upload a PDF and ask questions about it — powered by Ollama, LangChain, ChromaDB, and Streamlit. No API keys. No cloud. No cost.

---

## Live Demo

> *Watch the live demo video here — [link coming soon]*

---

## What it does

Upload any PDF → ask any question → get an AI-powered answer based on the document's content.

That's it. Simple, local, private.

---

##  Architecture

```
PDF Upload
    ↓
PyPDFLoader  →  Document chunks (RecursiveCharacterTextSplitter)
    ↓
nomic-embed-text  →  Vector Embeddings
    ↓
ChromaDB  →  Local Vector Store
    ↓
User Question  →  Semantic Retrieval  →  LLaMA 3.2 3B (via Ollama)
    ↓
Answer displayed in Streamlit UI
```

---

##  Tech Stack

| Component | Tool |
|---|---|
| UI | Streamlit |
| LLM | Ollama + LLaMA 3.2 3B |
| Embeddings | nomic-embed-text (via Ollama) |
| Vector Store | ChromaDB |
| Orchestration | LangChain (LCEL) |
| PDF Loader | PyPDFLoader |
| Text Splitter | RecursiveCharacterTextSplitter |

Everything runs **100% locally on your machine**. Your documents never leave your system.

---

## Project Structure

```
File_AI_Agent/
└── agent/
    ├── app.py            # Streamlit UI + main pipeline
    ├── pdf_loader.py     # Load PDF using PyPDFLoader
    ├── chunking.py       # Split documents into chunks
    ├── embedding.py      # Generate embeddings via Ollama
    ├── vector_store.py   # Store & retrieve from ChromaDB
    └── file_agent.py     # LLM chain using LangChain LCEL
```

---

## Getting Started

### Prerequisites

- Python 3.10+
- [Ollama](https://ollama.com) installed and running

### 1. Clone the repo

```bash
git clone https://github.com/yourusername/file-ai-agent.git
cd file-ai-agent/agent
```

### 2. Create a virtual environment

```bash
python -m venv venv
venv\Scripts\activate      # Windows
source venv/bin/activate   # Mac/Linux
```

### 3. Install dependencies

```bash
pip install streamlit langchain langchain-community langchain-ollama langchain-text-splitters chromadb pypdf
```

### 4. Pull required Ollama models

```bash
ollama pull llama3.2:3b
ollama pull nomic-embed-text
```

### 5. Run the app

```bash
streamlit run app.py
```

Open `http://localhost:8501` in your browser.

---

## How it works (RAG explained simply)

**RAG = Retrieval Augmented Generation**

Instead of asking the LLM to memorize your document (it can't), we:

1. **Chunk** the PDF into small overlapping pieces
2. **Embed** each chunk into a vector (a list of numbers representing meaning)
3. **Store** those vectors in ChromaDB
4. When you ask a question, **embed the question** too
5. **Find the most similar chunks** to your question
6. **Pass those chunks + your question** to the LLM
7. LLM answers **based only on the retrieved context**

This is how ChatGPT plugins, Notion AI, and most enterprise AI tools work under the hood.

---

## Known Limitations

- **It's a little slow** — since everything runs locally on CPU without a GPU, responses can take 15–60 seconds depending on your hardware. This is the trade-off for running it completely free and privately.
- For faster responses, use a GPU machine or swap to a smaller model like `tinyllama`.
- Large PDFs (100+ pages) may take longer to embed on first load.

---

## Possible Improvements

- [ ] Support for multiple file uploads
- [ ] Chat history / multi-turn conversation
- [ ] Support for `.docx` and `.txt` files
- [ ] GPU acceleration support
- [ ] Streaming responses for faster perceived speed
- [ ] Deploy to cloud with GPU (Replicate, Modal, etc.)

---

## What I learned building this

- How RAG pipelines work end-to-end
- LangChain LCEL (the modern way to build chains)
- Managing local LLMs with Ollama
- ChromaDB for vector storage and retrieval
- Streamlit session state for performance optimization
- Debugging Python dependency hell with LangChain versions 😅

---

## Key Files Explained

**`pdf_loader.py`** — Loads a PDF from disk using LangChain's `PyPDFLoader`, returns a list of `Document` objects.

**`chunking.py`** — Splits documents into 1000-character chunks with 200-character overlap so context isn't lost at boundaries.

**`embedding.py`** — Uses `nomic-embed-text` via Ollama to convert text chunks into vector embeddings locally.

**`vector_store.py`** — Creates a ChromaDB vector store from the embedded chunks, persisted to disk.

**`file_agent.py`** — Builds the RAG chain using LangChain LCEL: retriever → prompt → LLM → output parser.

**`app.py`** — Streamlit UI that ties everything together, with session state so the PDF isn't re-processed on every interaction.

---

## Contributing

PRs welcome! If you find a bug or want to add a feature, feel free to open an issue.

---

## 📄 License

MIT License — free to use, modify, and distribute.

---

## 🙌 Acknowledgements

- [Ollama](https://ollama.com) for making local LLMs dead simple
- [LangChain](https://langchain.com) for the RAG orchestration framework
- [ChromaDB](https://trychroma.com) for the local vector database
- [Streamlit](https://streamlit.io) for the fastest way to build AI UIs

---

*Built with ❤️ and a lot of debugging. If this helped you, drop a ⭐ on the repo!*
