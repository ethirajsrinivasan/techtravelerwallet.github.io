---
layout: post
title: Chat With Your PDFs Using LangChain and Flask
subtitle: Build an intelligent document Q&A system with OpenAI, Chroma, and Flask
categories: Tech
tags: [LangChain, Flask, OpenAI, ChromaDB, PDF, Chatbot, RetrievalQA]
excerpt: Learn how to create a smart app that lets users upload PDFs and ask questions, powered by GPT and vector search.
excerpt_image: /assets/images/docuchatai.png
---

![Chat With Your PDFs](/assets/images/docuchatai.png)


Have you ever wanted to *chat with a PDF*? In this post, we’ll show how to build a Flask app using **LangChain**, **ChromaDB**, and **OpenAI’s GPT** to do exactly that.

---

## 🧠 Key Components

- **LangChain** for document loading and QA
- **ChromaDB** for vector search
- **OpenAI** for embeddings and LLM
- **Flask** for the web interface

---

## 🖇️ PDF Upload and Processing

Use `PyPDFLoader` to read the PDF and split it into chunks:

```python
from langchain_community.document_loaders import PyPDFLoader

loader = PyPDFLoader(temp_pdf_path)
pages = loader.load_and_split()
```

Generate embeddings and store them in Chroma:

```python
from langchain_community.embeddings import OpenAIEmbeddings
from langchain_community.vectorstores import Chroma

embeddings = OpenAIEmbeddings(model="text-embedding-3-small")
vectordb = Chroma.from_documents(pages, embedding=embeddings)
```

---

## 💬 Asking Questions via RetrievalQA

Set up the retrieval-based QA pipeline:

```python
from langchain.chains import RetrievalQA
from langchain_openai import ChatOpenAI

qa_chain = RetrievalQA.from_chain_type(
llm=ChatOpenAI(model="gpt-3.5-turbo"),
retriever=vectordb.as_retriever(),
chain_type="stuff"
)

answer = qa_chain.invoke("What is this PDF about?")
```

---

## 🖥️ Flask Integration

Minimal route setup:

```python
@app.route('/upload', methods=['POST'])
def upload_pdf():
# Save, read, embed PDF
# Store vector DB in memory
```

```python
@app.route('/ask', methods=['POST'])
def ask_question():
# Get user query, invoke QA chain, return answer
```

---

## 🧪 Example

**Input:**
> What is the main finding of this report?

**Output:**
> The main finding of the report is that user engagement increased by 35% in Q2.

---

## 🧰 Requirements

```bash
pip install flask langchain langchain-openai langchain-community chromadb python-dotenv
```

---

## 🌐 Run It

```bash
export OPENAI_API_KEY=your_key
python app.py
```

Then go to `http://localhost:5500`.

---

✨ You now have a working "Chat with your PDF" app!
