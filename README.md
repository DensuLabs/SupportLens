<div align="center">

# 🔍 SupportLens

### AI-Powered Log Analysis

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.x-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)](https://streamlit.io)
[![Groq](https://img.shields.io/badge/Groq-Free_API-F55036?style=for-the-badge)](https://groq.com)
[![Ollama](https://img.shields.io/badge/Ollama-Local_AI-000000?style=for-the-badge)](https://ollama.com)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-pgvector-336791?style=for-the-badge&logo=postgresql&logoColor=white)](https://github.com/pgvector/pgvector)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

**Built by [Densu Labs]**

*100% free AI assistant that hunts thru your logs like a Team Lead*

</div>

## 📌 Overview

**SupportLens** is an open-source AI-powered log analysis and assistant built for IT and Application Support Analyst. Upload your **Windows Event Logs** (`.evtx`) or **Linux logs** (`.txt`), ask questions in plain English, and get expert-level answers in seconds — faster than any manual review.

Built on a fully **free stack**: Groq API for ultra-fast LLM inference, Ollama for local private embeddings, and PostgreSQL + pgvector for semantic search. **No data leaves your machine.**

---

## ⚡ Key Features

| Feature | Description |
|---|---|
| 📂 **Log ingestion** | Windows EVTX and Linux TXT log support |
| 🌐 **URL ingestion** | Scrape and index any web page into the knowledge base |
| 🧠 **RAG pipeline** | Logs are chunked, embedded locally, and stored in pgvector |
| ⚡ **Groq LLM** | Ultra-fast inference with LLaMA 3.3 70B via free Groq API |
| 🔒 **Private embeddings** | Fully local via Ollama — no data sent to the cloud |
| 💬 **Conversational memory** | Multi-turn chat with full history awareness |
| 🆓 **100% Free** | Groq API + Ollama + Docker — zero cost |

---

## 📥 Supported Log Formats

| Format | Extensions | Status |
|---|---|---|
| Windows Event Log | `.evtx` | ✅ Supported |
| Plain text / Linux logs | `.txt` | ✅ Supported |
| JSON logs | `.json` | 🔜 Roadmap |
| CSV logs | `.csv` | 🔜 Roadmap |
| XML event logs | `.xml` | 🔜 Roadmap |

---

## 🏗️ Architecture

```
User uploads log (EVTX / TXT) or pastes a URL
              │
              ▼
     Chunker (300 chars/chunk)
     Safe for nomic-embed-text 512-token limit
              │
              ▼
   Ollama (nomic-embed-text)  ──►  pgvector (PostgreSQL)
              │
              ▼
       User asks a question
              │
              ▼
   Semantic search in pgvector
              │
              ▼
   Groq LLM — LLaMA 3.3 70B
              │
              ▼
   Expert threat hunting answer 🎯
```

---

## 🧰 Tech Stack

| Component | Technology |
|---|---|
| UI | Streamlit |
| LLM | Groq API — `llama-3.3-70b-versatile` (free) |
| Embeddings | Ollama — `nomic-embed-text` (local, private) |
| Vector DB | PostgreSQL + pgvector (Docker) |
| RAG Framework | phidata |
| EVTX Parser | `python-evtx` |

---

## 🚀 Quick Start

### Prerequisites

- Python 3.10+
- [Docker](https://www.docker.com/)
- [Ollama](https://ollama.com/)
- A free [Groq API key](https://console.groq.com/keys) — no credit card needed

---

### 1. Clone the repo

```bash
git clone https://DensuLabs/SupportLens.git
cd supportlens
```

### 2. Start the database

```bash
docker run -d --name supportlens-db --restart always \
  -e POSTGRES_DB=ai \
  -e POSTGRES_USER=ai \
  -e POSTGRES_PASSWORD=ai \
  -p 5532:5432 ankane/pgvector
```

### 3. Start Ollama and pull the embedding model

```bash
ollama serve
ollama pull nomic-embed-text
```

### 4. Python setup

```bash
python3 -m venv venv
source venv/bin/activate       # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 5. Set your free Groq API key

```bash
export GROQ_API_KEY="your_free_key_here"
```

Or create a `.env` file:

```env
GROQ_API_KEY=your_free_key_here
```

### 6. Launch ThreatLens

```bash
streamlit run app.py
```

Open your browser at **http://localhost:8501** 🚀

---

## 📁 Project Structure

```
supportlens/
├── app.py              # Streamlit UI + file readers + chunking logic
├── assistant.py        # AI brain — Groq + RAG + SOC analyst prompts
├── requirements.txt    # Python dependencies
├── assets/             # Banner and media files
├── LICENSE             # MIT License
└── README.md
```

---

## 📦 requirements.txt

```
streamlit>=1.35.0
phidata>=2.4.0
groq>=0.9.0
ollama>=0.2.0
pgvector>=0.2.5
psycopg[binary]>=3.1.0
sqlalchemy>=2.0.0
python-evtx>=0.7.4
evtx>=0.8.2
requests>=2.31.0
beautifulsoup4>=4.12.0
openai>=1.0.0
```

---

## 🧪 Test Datasets

Don't have logs to test with? Here are great free resources:

**Windows EVTX:**

- [evtx-baseline](https://github.com/NextronSystems/evtx-baseline) — baseline vs anomaly EVTXs

**Linux Logs (TXT):**

- [Loghub](https://github.com/logpai/loghub) — SSH, syslog, auth.log samples
- [SecRepo](http://www.secrepo.com/) — Apache, DNS, IDS logs

**From your own machine:**

```bash
cp /var/log/auth.log ~/test_auth.txt
cp /var/log/syslog   ~/test_syslog.txt
dmesg > ~/test_dmesg.txt
```

---

## 🎯 Example Questions

Once you upload a log file, try asking:

```
What failed login attempts are in this log?
What happened between 2:00 AM and 3:00 AM?
```

---

## 🤝 Contributing

Contributions are welcome from the community! Feel free to open issues or pull requests for:

- New log format support (JSON, CSV, XML, Syslog)
- Better chunking strategies
- Dashboard / visualization features
- Bug fixes and improvements

---

## 📄 License

MIT — see [LICENSE](LICENSE) for details.

---

<div align="center">

**👨‍💻 Built by [Densu Labs]**

*"Analyze faster. Find smarter. Stay ahead."* 🛡️

</div>
