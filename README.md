# SkillNova Intern Support Chatbot

An AI-powered support chatbot for interns on the SkillNova / UptoSkills platform.
Built using a Retrieval-Augmented Generation (RAG) architecture — answers questions
exclusively from official organisation documents, no hallucination.

---

## What it does

- Answers intern queries about attendance, meetings, tasks, guidelines, and performance
- Retrieves answers only from 5 official UptoSkills knowledge base documents
- Rejects off-topic questions with a polite fallback message
- Supports multi-turn conversation (remembers last 6 exchanges)

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| SentenceTransformers (all-MiniLM-L6-v2) | Convert text to embeddings |
| FAISS | Fast similarity search across document chunks |
| Groq API (Llama 3.1 8B Instant) | Generate answers from retrieved context |
| Streamlit | Web-based chat interface |
| python-dotenv | Secure API key management |
| python-docx | Read .docx knowledge base files |

---

## Project Structure
```
├── app.py                  # Main Streamlit app (run this)
├── chatbot.py              # Terminal version (for reference)
├── .env                    # Your API key (never share this)
├── .env.example            # Template for .env
└── knowledge_base/
    ├── Attendance_Policy.txt
    ├── Internship_Guidelines.txt
    ├── Meeting_Scheduling_Process.txt
    ├── Task_Validation_Rules.txt
    └── Intern_Performance_Evaluation.txt
```

---

## Setup & Run

**1. Install dependencies**
```bash
pip install streamlit sentence-transformers faiss-cpu groq python-dotenv python-docx
```

**2. Add your Groq API key**

Create a `.env` file in the project folder:
```
GROQ_API_KEY=your_actual_key_here
```
Get a free key at: https://console.groq.com

**3. Run the app**
```bash
streamlit run app.py
```
Opens at `http://localhost:8501`

> First run downloads the embedding model — subsequent runs are instant.

---

## How it works
```
Startup:   Load Docs → Chunk Text → Generate Embeddings → Build FAISS Index
Query:     Embed Question → Search FAISS → Filter by Distance → Generate Answer
```

1. Documents are split into 150-word chunks with 30-word overlap
2. Each chunk is converted to a 384-dimensional vector using SentenceTransformers
3. User question is embedded and compared against all chunks via L2 distance
4. Top 5 relevant chunks (distance < 2.0) are sent to Groq LLM as context
5. LLM answers strictly from provided context only

---

## Sample Results

| Question | Result |
|----------|--------|
| What should I do if I'm sick? | PASS |
| How is my performance evaluated? | PASS |
| What is the capital of France? | PASS — Fallback (not in KB) |

---

## Built for

UptoSkills AI Intern Team — Task 3  
Submitted: March 20, 2026
