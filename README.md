<p align="center">
  <img src="./assets/banner.svg" alt="SarkariSahayak Banner" width="100%"/>
</p>

<p align="center">
  <a href="#"><img src="https://img.shields.io/badge/Built%20at-HackBlr%202026-0D9488" alt="HackBlr 2026"/></a>
  <a href="https://vapi.ai"><img src="https://img.shields.io/badge/Powered%20by-Vapi-14B8A6" alt="Vapi"/></a>
  <a href="https://qdrant.tech"><img src="https://img.shields.io/badge/Powered%20by-Qdrant-DC382D" alt="Qdrant"/></a>
  <a href="https://fastapi.tiangolo.com"><img src="https://img.shields.io/badge/Backend-FastAPI-009688" alt="FastAPI"/></a>
  <a href="https://www.python.org"><img src="https://img.shields.io/badge/Python-3.10+-3776AB" alt="Python"/></a>
  <a href="#-license"><img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="License: MIT"/></a>
</p>

---

## 📖 About

**SarkariSahayak** (सरकारी सहायक — *"Government Helper"*) is a voice-first AI agent that helps Indian citizens discover, understand, and apply for government welfare schemes through natural conversation — no app, no typing, no internet literacy required.

Just call a number, speak naturally in Hindi or English, and the agent will:

- ✅ Check your **eligibility** across multiple schemes based on your profile
- 📄 Explain the **documents** you'll need to apply
- 🧭 Guide you through the **application** process

### 🎯 The Problem We Solve

- **53%** of India's salaried workforce lacks social security benefits
- **652 million** Indians are still offline (44.7% of the population)
- Only **25%** of rural India is digitally literate
- **700+** welfare schemes exist, but most eligible citizens never access them

Literacy, language, and digital barriers prevent millions from accessing benefits they deserve. SarkariSahayak bridges this gap with voice — the most natural, universally accessible interface.

---

## 🎬 Demo

> _Screenshot / demo GIF placeholder — add here after recording_

<!--
Recommended captures to add here:
1. A GIF of an actual call (use a screen recorder while dialing the Vapi number)
2. A screenshot of the Vapi dashboard showing the assistant configuration
3. A screenshot of a real call transcript
Save them to assets/ and reference like:
<p align="center"><img src="./assets/demo.gif" width="600"/></p>
-->

📹 **Demo video:** _Coming soon_
📞 **Try it live:** _Demo phone number will be shared at HackBlr 2026_

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

## 🔄 How It Works

<p align="center">
  <img src="./assets/flow.svg" alt="User Flow - 4 Steps" width="100%"/>
</p>

1. **User Calls** — The user dials the SarkariSahayak number and speaks naturally in Hindi or English.
2. **AI Understands** — Vapi transcribes the speech in real-time and passes the query to our FastAPI backend.
3. **Qdrant Retrieves** — The backend queries Qdrant, which uses vector search to find the most relevant schemes based on the user's profile and needs.
4. **Agent Responds** — The LLM composes a helpful response, Vapi converts it to speech, and the user hears a natural voice reply.

---

## 🏗️ Architecture

<p align="center">
  <img src="./assets/architecture.svg" alt="System Architecture" width="100%"/>
</p>

**Data flow:** Voice In → Vapi (STT) → FastAPI (Logic) → Qdrant (Retrieval) + LLM (Reasoning) → FastAPI (Response) → Vapi (TTS) → Voice Out

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Voice** | [Vapi](https://vapi.ai) | Real-time STT/TTS, bilingual voice handling, webhooks |
| **Intelligence** | [Qdrant](https://qdrant.tech) | Vector database for semantic scheme retrieval |
| **Backend** | [FastAPI](https://fastapi.tiangolo.com) | Python async server, business logic, orchestration |
| **LLM** | OpenAI / Anthropic | Natural language understanding & generation |
| **Language** | Python 3.10+ | Core programming language |
| **Deployment** | Railway / Render | Backend hosting |

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
├── assets/                     # README images & demo media
│   ├── banner.svg
│   ├── architecture.svg
│   └── flow.svg
├── data/                       # Scheme data as JSON
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

## 👥 Team Batman

Built with ❤️ at **HackBlr 2026** by:

| Member | Role |
|--------|------|
| **Aarushi Sen** | _Add role_ |
| **Bhoomika Mittal** | _Add role_ |
| **Jagrit Goel** | _Add role_ |
| **Parth Krishan Goswami** | _Add role_ |

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
