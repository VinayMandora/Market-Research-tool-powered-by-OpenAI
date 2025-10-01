# Market Research Tool 📈

A lightweight **Streamlit** app that turns any set of news/article URLs into a **retrieval-augmented Q&A (RAG)** assistant.  
It fetches pages, splits them into chunks, builds **OpenAI embeddings**, stores them in a **FAISS** vector index, and answers your questions with **sources**.

> ⚠️ Uses OpenAI APIs via LangChain. You’ll need an `OPENAI_API_KEY`.

---

## ✨ Features

- 🧭 Input up to **3 URLs** in the sidebar
- 📄 Fetch & parse content using `UnstructuredURLLoader`
- 🔪 Chunk with `RecursiveCharacterTextSplitter` (chunk_size=1000)
- 🧠 Embed with **OpenAIEmbeddings** and store in **FAISS**
- 🤖 Ask questions; get **grounded answers** with **source citations**
- 💾 Caches the FAISS store to `faiss_store_openai.pkl`

---

## 🗂 Project Structure (suggested)

```
Market Research Tool/
├─ app.py                 # your Streamlit app (the code you shared)
├─ requirements.txt
├─ .env                   # contains OPENAI_API_KEY (never commit!)
└─ faiss_store_openai.pkl # auto-created after "Process URLs"
```

---

## ✅ Requirements

Tested with **Python 3.10**. Create a `requirements.txt` like this:

```
streamlit==1.36.0
langchain==0.0.350        # legacy import path compatible with your code
openai==0.28.1
faiss-cpu==1.7.4
unstructured==0.10.30
beautifulsoup4==4.12.3
lxml==5.2.2
tiktoken==0.7.0
python-dotenv==1.0.1
requests==2.32.3
```

> 💡 If you prefer **modern LangChain (>=0.1)**, see **Compatibility Notes** below and adjust imports.

---

## 🔐 Environment Variables

Create a `.env` file in the project root:

```
OPENAI_API_KEY=sk-********************************
```

> Never commit `.env` to Git. Add it to `.gitignore`.

---

## ⚙️ Setup & Run

```bash
# 1) Create and activate a venv
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS/Linux
source .venv/bin/activate

# 2) Install dependencies
pip install -r requirements.txt

# 3) Run Streamlit
streamlit run app.py
```

Open the local URL Streamlit prints (usually `http://localhost:8501`).

---

## 🧭 How to Use

1. Paste up to **3 article/news URLs** into the sidebar (left).
2. Click **“Process URLs”**  
   - You’ll see status messages: “Data Loading…”, “Text Splitter…”, “Embedding Vector…”
   - A FAISS index is saved to `faiss_store_openai.pkl`.
3. In the main panel, type your **question** (e.g., *“What are the key takeaways about inflation?”*).
4. Read the **Answer** and review the **Sources** list.

---

## 🧠 How It Works (RAG Flow)

1. **Load** pages from URLs (`UnstructuredURLLoader`)
2. **Split** text into chunks (`RecursiveCharacterTextSplitter`)
3. **Embed** chunks (`OpenAIEmbeddings`)
4. **Index** in vector store (FAISS)
5. **Retrieve** top chunks for a query
6. **Generate** answer with **`RetrievalQAWithSourcesChain`** (OpenAI LLM)
7. **Cite** source URLs under the answer

---

## 🧪 Troubleshooting

- **`OPENAI_API_KEY` not found**  
  Make sure `.env` exists and contains your key; restart the app.

- **`AttributeError: module 'langchain' has no attribute 'OpenAI'`**  
  You’re likely on a newer LangChain version. See **Compatibility Notes** below.

- **Unstructured loader issues on some pages**  
  Dynamic/JS-heavy or paywalled pages may not parse well. Try alternate links.

- **Large pages slow embedding**  
  Reduce `chunk_size`, or limit to fewer/shorter URLs.

---

## 🧩 Compatibility Notes (Modern LangChain)

Your code uses legacy imports like:
```python
from langchain import OpenAI
from langchain.embeddings import OpenAIEmbeddings
```

If you upgrade to **LangChain >= 0.1**, switch to the newer packages:

```python
# pip install langchain langchain-openai faiss-cpu
from langchain_openai import OpenAI, OpenAIEmbeddings
```

Everything else (Streamlit, FAISS, chain wiring) remains conceptually the same.

---

## 🔒 Security & Privacy

- Don’t log or display your API key.
- Content from URLs is embedded and stored locally in `faiss_store_openai.pkl`.
- Be mindful of website terms and data sensitivity.

---

## 🗺️ Roadmap / Ideas

- Add support for **more than 3 URLs**
- Let users **upload PDFs** (use `PyPDFLoader`)
- Persist **multiple named indexes** (e.g., per topic)
- Add **model & retriever settings** (k, temperature) in the sidebar
- Deploy to **Streamlit Community Cloud** or **Docker**

---

## 👤 Author

**Vinay Mandora** · Manchester, UK
