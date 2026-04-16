# 🎙️ SarkariSahayak

> **Voice AI Agent for Government Scheme Accessibility** — Making Indian welfare schemes accessible through natural conversation, in Hindi & English.

[![Built at HackBlr 2026](https://img.shields.io/badge/Built%20at-HackBlr%202026-0D9488)]()
[![Powered by Vapi](https://img.shields.io/badge/Powered%20by-Vapi-14B8A6)](https://vapi.ai)
[![Powered by Qdrant](https://img.shields.io/badge/Powered%20by-Qdrant-DC382D)](https://qdrant.tech)
[![FastAPI](https://img.shields.io/badge/Backend-FastAPI-009688)](https://fastapi.tiangolo.com)
[![Python](https://img.shields.io/badge/Python-3.10+-3776AB)](https://www.python.org)

---

## 📖 About

**SarkariSahayak** (सरकारी सहायक — "Government Helper") is a voice-first AI agent that helps Indian citizens discover, understand, and apply for government welfare schemes through natural conversation — no app, no typing, no internet literacy required.

Just call a number, speak naturally in Hindi or English, and the agent will:
- Check your **eligibility** across multiple schemes based on your profile
- Explain the **documents** you'll need to apply
- Guide you through the **application** process

### 🎯 The Problem We Solve

- **53%** of India's salaried workforce lacks social security benefits
- **652 million** Indians are still offline (44.7% of the population)
- Only **25%** of rural India is digitally literate
- **700+** welfare schemes exist, but most eligible citizens never access them

Literacy, language, and digital barriers prevent millions from accessing benefits they deserve. SarkariSahayak bridges this gap with voice — the most natural, universally accessible interface.

---

## ✨ Features

- 🎤 **Voice-First Interaction** — Natural conversation, no typing required
- 🌐 **Bilingual Support** — Seamless Hindi & English with code-switching
- ✅ **Real-time Eligibility Checking** — Context-aware assessment across schemes
- 📄 **Document Requirement Guidance** — Clear list of documents needed per scheme
- 🧠 **Qdrant-Powered Smart Retrieval** — Semantic search matches user profile to eligible schemes
- 📞 **Zero-Barrier Access** — Works on any phone; no smartphone or app required
- 🔄 **Scalable Architecture** — Add new schemes without retraining models

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Voice** | [Vapi](https://vapi.ai) | Real-time STT/TTS, bilingual voice handling, webhooks |
| **Intelligence** | [Qdrant](https://qdrant.tech) | Vector database for semantic scheme retrieval |
| **Backend** | [FastAPI](https://fastapi.tiangolo.com) | Python async server, business logic, orchestration |
| **Language** | Python 3.10+ | Core programming language |
| **Deployment** | Railway / Render | Backend hosting |

---

## 🏗️ Architecture

```
    ┌─────────────┐      ┌──────────────┐      ┌──────────────┐
    │             │      │              │      │              │
    │   User      │─────▶│     Vapi     │─────▶│   FastAPI    │
    │  (Phone)    │ voice│  (STT/TTS)   │ JSON │  (Backend)   │
    │             │◀─────│              │◀─────│              │
    └─────────────┘      └──────────────┘      └──────┬───────┘
                                                      │
                                                      │ semantic query
                                                      ▼
                                               ┌──────────────┐
                                               │    Qdrant    │
                                               │  (Vector DB) │
                                               │              │
                                               └──────────────┘
```

**Flow:** Voice In → Vapi (STT) → FastAPI (Logic) → Qdrant (Retrieval) → FastAPI (Response) → Vapi (TTS) → Voice Out

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10 or higher
- A [Vapi account](https://vapi.ai) (free tier works)
- A [Qdrant Cloud account](https://qdrant.tech) (free tier) or self-hosted Qdrant
- An LLM API key (OpenAI / Anthropic / Groq)
- `ngrok` for local webhook testing ([download](https://ngrok.com))

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/<your-username>/sarkari-sahayak.git
   cd sarkari-sahayak
   ```

2. **Create and activate a virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate   # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure environment variables**

   Create a `.env` file in the project root:
   ```env
   # Vapi
   VAPI_API_KEY=your_vapi_private_key
   VAPI_PUBLIC_KEY=your_vapi_public_key

   # Qdrant
   QDRANT_URL=https://your-cluster.qdrant.io
   QDRANT_API_KEY=your_qdrant_api_key
   QDRANT_COLLECTION=schemes

   # LLM
   OPENAI_API_KEY=your_openai_key
   # or ANTHROPIC_API_KEY=your_anthropic_key

   # App
   APP_HOST=0.0.0.0
   APP_PORT=8000
   ```

5. **Seed the Qdrant collection with scheme data**
   ```bash
   python scripts/seed_qdrant.py
   ```

6. **Run the FastAPI server**
   ```bash
   uvicorn main:app --reload --port 8000
   ```

7. **Expose your local server with ngrok** (for Vapi webhooks)
   ```bash
   ngrok http 8000
   ```
   Copy the `https://...ngrok-free.app` URL and set it as your **Server URL** in the Vapi assistant settings.

8. **Test a call** from the Vapi dashboard or call the linked phone number.

---

## 📁 Project Structure

```
sarkari-sahayak/
├── app/
│   ├── main.py                 # FastAPI app entry point
│   ├── routes/
│   │   ├── vapi_webhook.py     # Handles Vapi events & function calls
│   │   └── health.py
│   ├── services/
│   │   ├── eligibility.py      # Eligibility matching engine
│   │   ├── qdrant_client.py    # Vector DB wrapper
│   │   └── llm.py              # LLM orchestration
│   ├── models/
│   │   └── schemas.py          # Pydantic models
│   └── config.py
├── data/
│   ├── ayushman_bharat.json
│   ├── national_scholarship.json
│   ├── pm_awas_yojana.json
│   ├── pm_kisan.json
│   └── pm_mudra_yojana.json
├── scripts/
│   └── seed_qdrant.py          # One-time data seeding script
├── tests/
├── .env.example
├── requirements.txt
├── README.md
└── LICENSE
```

---

## 🏛️ Schemes Covered (Demo v1)

| Scheme | Sector | Benefit |
|--------|--------|---------|
| **Ayushman Bharat** | Health | Health insurance up to ₹5 lakh |
| **National Scholarship** | Education | Financial aid for meritorious students |
| **PM Awas Yojana** | Housing | Affordable housing for EWS/LIG/MIG |
| **PM Kisan** | Agriculture | ₹6,000/year for farmer families |
| **PM Mudra Yojana** | Finance | Collateral-free loans up to ₹20L |

> 📌 These 5 schemes represent our MVP scope. See the [Roadmap](#-roadmap) for planned expansion.

---

## 🗺️ Roadmap

- [x] **v1 (Demo)** — 5 foundational schemes, Hindi + English
- [ ] **v2** — Add 3 regional languages (Tamil, Bengali, Marathi)
- [ ] **v3** — Expand to 50+ central & state government schemes
- [ ] **v4** — Full application-filling assistance via voice
- [ ] **v5** — Integration with DigiLocker & myScheme APIs
- [ ] **v6** — WhatsApp & IVR channel support
- [ ] **v7** — Support for all 22 official Indian languages

---

## 🎬 Demo

📹 **Demo video:** _Coming soon_
📞 **Try it live:** _Demo phone number will be shared at HackBlr 2026_

---

## 👥 Team Batman

Built with ❤️ at **HackBlr 2026** by:

- **Aarushi Sen**
- **Bhoomika Mittal**
- **Jagrit Goel**
- **Parth Krishan Goswami**

---

## 🤝 Contributing

This is a hackathon project, but we welcome contributions! If you'd like to help extend scheme coverage, add new languages, or improve the eligibility engine:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-scheme`)
3. Commit your changes (`git commit -m 'Add scheme XYZ'`)
4. Push to the branch (`git push origin feature/new-scheme`)
5. Open a Pull Request

---

## 📜 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **[HackBlr 2026](https://hackblr.com)** — For the platform and problem statement
- **[Vapi](https://vapi.ai)** — For the voice AI infrastructure
- **[Qdrant](https://qdrant.tech)** — For the vector database
- **Government of India** — For the welfare schemes that inspire this work
- Every citizen who deserves access to the rights they're entitled to

---

<p align="center">
  <i>"Because every citizen deserves access to their rights."</i>
</p>
