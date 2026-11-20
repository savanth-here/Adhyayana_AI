# AdhyayanaAI – Cognitive-State-Aware Multimodal AI Tutoring System

A production-grade B.Tech Final Year Project / Research Prototype that combines **real-time cognitive state analysis**, **adaptive tutoring**, **multilingual voice interaction**, and a **modern full-stack web application**.

---

## Project Structure

```
adhyayana-ai/
├── backend/                  # FastAPI Python backend
│   ├── app/
│   │   ├── main.py           # FastAPI application entry point
│   │   ├── config.py         # Settings & environment variables
│   │   ├── database.py       # SQLAlchemy SQLite setup
│   │   ├── models/           # Database models (Student, Session, etc.)
│   │   ├── schemas/          # Pydantic request/response schemas
│   │   ├── routers/          # API route handlers
│   │   │   ├── auth.py       # Sign up / Login (JWT)
│   │   │   ├── students.py   # Student profile & leaderboard
│   │   │   ├── tutor.py      # AI chat endpoint
│   │   │   ├── cognitive.py  # Cognitive state analysis
│   │   │   ├── analytics.py  # Learning analytics
│   │   │   ├── quiz.py       # Adaptive quiz generation
│   │   │   └── voice.py      # ASR + TTS endpoints
│   │   ├── services/         # Business logic
│   │   │   ├── ai_service.py        # OpenAI GPT integration
│   │   │   ├── cognitive_service.py # Heuristic + ML cognitive detection
│   │   │   ├── adaptive_service.py  # Pedagogical strategy selection
│   │   │   ├── voice_service.py     # Whisper ASR + gTTS/OpenAI TTS
│   │   │   └── auth_utils.py        # JWT token utilities
│   │   └── ml/               # Machine Learning pipeline
│   │       ├── generate_data.py   # Synthetic dataset (1000 students)
│   │       ├── cognitive_model.py # RF + XGBoost ensemble
│   │       └── train.py           # Training script
│   ├── requirements.txt
│   ├── .env.example
│   └── Dockerfile
│
├── frontend/                 # Next.js 14 TypeScript frontend
│   ├── src/
│   │   ├── app/
│   │   │   ├── page.tsx           # Landing page
│   │   │   ├── layout.tsx         # Root layout
│   │   │   ├── globals.css        # Tailwind + custom styles
│   │   │   ├── auth/
│   │   │   │   ├── login/         # Login page
│   │   │   │   └── signup/        # Signup page
│   │   │   └── dashboard/
│   │   │       ├── page.tsx       # Dashboard home
│   │   │       ├── tutor/         # AI Tutor Chat
│   │   │       ├── cognitive/     # Cognitive State Analysis
│   │   │       ├── analytics/     # Learning Analytics
│   │   │       └── learning/      # Adaptive Quiz Engine
│   │   ├── components/
│   │   │   ├── layout/            # Sidebar, TopBar
│   │   │   └── ...                # Feature-specific components
│   │   ├── hooks/                 # useAuth, useChat
│   │   ├── lib/                   # api.ts, auth.ts, utils.ts
│   │   └── types/                 # TypeScript interfaces
│   ├── package.json
│   ├── tailwind.config.ts
│   ├── next.config.ts
│   └── Dockerfile
│
├── cognitive/                # Original cognitive state classifier (DistilBERT)
├── knowledge/                # BKT & DKT knowledge tracing
├── pipeline/                 # SRS pipeline (Whisper→GPT→TTS)
├── docker-compose.yml
└── README.md
```

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 14, React 18, TypeScript, Tailwind CSS, Framer Motion |
| Charts | Recharts (Line, Bar, Area, Radar, Pie charts) |
| Backend | FastAPI, Python 3.11, Uvicorn |
| Database | SQLite (prototype) / PostgreSQL (production) |
| ORM | SQLAlchemy 2.0 |
| Auth | JWT (python-jose) + bcrypt |
| AI/LLM | OpenAI GPT-4o |
| ASR | OpenAI Whisper API / local whisper model |
| TTS | gTTS / OpenAI TTS |
| ML | Scikit-learn, XGBoost, Random Forest |
| Deep Learning | PyTorch, Transformers (DistilBERT) |
| Deployment | Docker, Docker Compose, Vercel (frontend), Render (backend) |

---

## Quick Start

### Prerequisites
- Python 3.11+
- Node.js 20+
- OpenAI API key (optional — app runs with mock responses without it)

### Backend Setup

```bash
cd backend

# Create virtual environment
python -m venv venv
venv\Scripts\activate        # Windows
source venv/bin/activate     # Linux/Mac

# Install dependencies
pip install -r requirements.txt

# Configure environment
copy .env.example .env
# Edit .env and add your OPENAI_API_KEY

# (Optional) Train ML model
python -m app.ml.train

# Start API server
uvicorn app.main:app --reload --port 8000
```

API docs available at: http://localhost:8000/docs

### Frontend Setup

```bash
cd frontend

# Install dependencies
npm install --legacy-peer-deps

# Configure environment
copy .env.local.example .env.local
# Edit .env.local: NEXT_PUBLIC_API_URL=http://localhost:8000

# Start development server
npm run dev
```

Frontend available at: http://localhost:3000

---

## Docker Deployment

```bash
# Copy and configure environment
copy backend\.env.example .env
# Add OPENAI_API_KEY to .env

# Build and start all services
docker-compose up --build

# Access:
# Frontend: http://localhost:3000
# Backend API: http://localhost:8000
# API Docs: http://localhost:8000/docs
```

---

## AI Pipeline

```
Student Voice/Text
       │
       ▼
  Whisper ASR (OpenAI/local)
       │
       ▼
  Language Detection (en/te/hi)
       │
       ▼
  Cognitive State Analysis
  ┌────────────────────────┐
  │ Text keyword heuristics│
  │ + ML feature model     │
  │ → confused/engaged/    │
  │   retained/neutral     │
  └────────────────────────┘
       │
       ▼
  Adaptive Strategy Selection
  ┌────────────────────────────────────────┐
  │ confusion>70% → Simplify + Examples    │
  │ engagement<40% → Gamify               │
  │ retention<50% → Schedule Revision     │
  │ fatigue>80% → Suggest Break           │
  └────────────────────────────────────────┘
       │
       ▼
  GPT-4o Response Generation
  (system prompt includes strategy)
       │
       ▼
  gTTS / OpenAI TTS Audio Output
       │
       ▼
  Student Hears Adaptive Response
```

---

## ML Cognitive Model

**Training Data:** 10,000 synthetic student sessions with features:
- Response time, quiz accuracy, hint usage, repeated mistakes, session duration, difficulty level

**Labels:** confused, engaged, retained, neutral

**Model:** Voting ensemble of Random Forest + Gradient Boosting + XGBoost

**Training:**
```bash
cd backend
python -m app.ml.train
```

---

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /api/auth/signup | Register new student |
| POST | /api/auth/login | Login (returns JWT) |
| GET | /api/students/me | Get current student profile |
| GET | /api/students/me/stats | Learning statistics |
| GET | /api/students/leaderboard | XP leaderboard |
| POST | /api/tutor/chat | AI tutoring chat |
| GET | /api/tutor/history | Chat history |
| POST | /api/cognitive/analyze | Analyze cognitive state |
| GET | /api/cognitive/latest | Latest cognitive state |
| GET | /api/analytics/dashboard | Dashboard stats |
| GET | /api/analytics/weekly-performance | 7-day trends |
| GET | /api/analytics/weak-topics | Weak/strong topics |
| POST | /api/quiz/generate | Generate adaptive quiz |
| POST | /api/quiz/submit | Submit quiz results |
| POST | /api/voice/transcribe | Speech to text |
| POST | /api/voice/synthesize | Text to speech |
| POST | /api/voice/process | Full voice pipeline |

---

## Features

- **Cognitive State Detection** — Real-time confusion, engagement, retention, fatigue analysis
- **Adaptive Tutoring** — GPT-4o adjusts explanation depth based on your state
- **Multilingual** — English, Telugu, Hindi voice + text support
- **Voice Interaction** — Whisper ASR + TTS for full voice sessions
- **Learning Analytics** — Weekly trends, subject performance, retention graphs
- **Adaptive Quizzes** — AI-generated MCQs calibrated to your mastery level
- **Gamification** — XP points, badges, day streaks, leaderboard
- **Knowledge Tracing** — BKT + DKT algorithms track skill mastery
- **Dark UI** — Glassmorphism design with smooth Framer Motion animations

---

## Deployment

### Frontend → Vercel
```bash
cd frontend
npx vercel --prod
```
Set environment variable: `NEXT_PUBLIC_API_URL=https://your-backend.onrender.com`

### Backend → Render
1. Create new Web Service on render.com
2. Set Build Command: `pip install -r requirements.txt`
3. Set Start Command: `uvicorn app.main:app --host 0.0.0.0 --port $PORT`
4. Add environment variables from `.env.example`

---

## Research Context

This system is designed as a **B.Tech Final Year Project** and **Conference Research Prototype** demonstrating:
1. Real-time cognitive state prediction using NLP + behavioral signals
2. LLM-based adaptive pedagogy with strategy selection
3. Multilingual speech-to-reasoning-to-speech educational pipeline
4. Bayesian Knowledge Tracing for skill mastery estimation

---

*Built with Claude Code · AdhyayanaAI Team · 2026*
