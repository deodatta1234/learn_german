# telc Deutsch Trainer

  An AI-powered German learning app covering A1–C1 CEFR levels with authentic telc exam preparation. Teaches from your own PDF books via RAG,
  tracks vocabulary with spaced repetition, corrects writing inline, and evaluates spoken German via audio recording.

  ## Features

  - **Five telc exam sections** — Reading, Listening, Writing, Speaking, Sprachbausteine (Grammar cloze)
  - **RAG from your own PDFs** — upload any German textbook; the AI teaches directly from it
  - **Spaced repetition flashcards** — SM-2 algorithm, auto-extracts vocabulary after each lesson
  - **Writing correction** — inline error markup + scores across task/vocab/grammar/coherence (0–5 each)
  - **Speaking practice** — audio recording → OpenAI transcription → AI evaluation
  - **Learner profile** — per-user level (A1–C1), target exam, and weak-area tracking injected into every prompt
  - **Language modes** — Bilingual (default), English-only, German soft immersion
  - **Dark/light mode** — preference persisted in localStorage
  - **Cross-device sync** — all data in Supabase; works on phone, laptop, tablet

  ## Tech Stack

  | Layer | Technology |
  |---|---|
  | Frontend | React 19 + Vite + Tailwind CSS 4 |
  | Routing | React Router DOM 7 |
  | i18n | i18next + react-i18next |
  | PWA | manifest.json (installable on mobile) |
  | Backend | FastAPI (Python 3.11+) + Uvicorn |
  | AI — text | OpenAI gpt-5.4-mini |
  | AI — audio | OpenAI gpt-4o-transcribe |
  | AI — TTS | gTTS (German listening audio) |
  | RAG — PDF (digital) | PyMuPDF (fitz) |
  | RAG — PDF (scanned) | gpt-5.4-mini vision + pdf2image |
  | RAG — vector DB | Weaviate (hybrid search + cross-encoder reranking) |
  | RAG — embeddings | text2vec-openai (via Weaviate module) |
  | Auth + Database | Supabase (PostgreSQL + Email/Google Auth) |
  | Frontend hosting | Vercel |
  | Backend hosting | Google Cloud Run |
  | Vector DB hosting | Hetzner CX23, Nuremberg, Germany |

  ## Project Structure
  ```
  telc-app/
  ├── backend/
  │   ├── main.py                      # FastAPI app, all routes
  │   ├── database.py                  # Supabase client singleton
  │   ├── schema.sql                   # Supabase DB schema
  │   ├── requirements.txt
  │   ├── Dockerfile
  │   ├── auth/
  │   │   └── middleware.py            # Supabase JWT verification
  │   ├── rag/
  │   │   ├── ingest.py                # Orchestrate PDF → chunks → Weaviate
  │   │   ├── retriever.py             # Hybrid search + reranking
  │   │   ├── chunker.py               # 300-word chunk splitter
  │   │   ├── detector.py              # Detect digital vs scanned PDF
  │   │   ├── extractor_digital.py     # PyMuPDF text extraction
  │   │   └── extractor_scanned.py     # Vision OCR via OpenAI
  │   └── tests/
  │       ├── test_main.py
  │       ├── test_generate_exercise.py
  │       ├── test_prompts.py
  │       └── test_rag.py
  ├── frontend/
  │   ├── vite.config.js
  │   ├── package.json
  │   ├── Dockerfile
  │   └── src/
  │       ├── App.jsx
  │       ├── auth/
  │       │   └── Login.jsx
  │       ├── components/
  │       │   ├── Layout.jsx
  │       │   ├── Sidebar.jsx
  │       │   └── Header.jsx
  │       ├── sections/
  │       │   ├── Home.jsx
  │       │   ├── Chat.jsx
  │       │   ├── Reading.jsx
  │       │   ├── Listening.jsx
  │       │   ├── Writing.jsx
  │       │   ├── Speaking.jsx
  │       │   ├── Grammar.jsx
  │       │   └── Vocab.jsx
  │       ├── contexts/
  │       │   └── ThemeContext.jsx
  │       ├── lib/
  │       │   ├── supabase.js
  │       │   └── chatService.js
  │       └── i18n/
  │           ├── en.json
  │           ├── de.json
  │           └── index.js
  ├── data/
  │   ├── pdfs/                        # Study PDFs (gitignored)
  │   └── audio/                       # Speaking recordings (gitignored)
  ├── docker-compose.yml
  └── .env.example
  ```
