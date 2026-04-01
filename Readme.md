## Kenya Medical Assistant (Frontend)

Static HTML/CSS/JS frontend for the RAG-powered Kenyan medical chatbot.
Hosted on **Vercel**. Connects to the Django/PostgreSQL backend on **Render**.

---

## Features

| Feature | Details |
|---|---|
| **Chat** | RAG symptom matching with markdown-rendered first aid |
| **Emergency flow** | Auto-detects emergency keywords → requests GPS → live Leaflet map + nearby hospitals via OpenStreetMap |
| **Murang'a Hospitals tab** | Embedded Google Maps showing all hospitals & clinics in Murang'a County |
| **Persistent history** | Last 80 messages stored per session in `localStorage` |
| **Feedback** | Emoji star rating modal after each diagnosis |
| **Quick actions** | Sidebar buttons for 6 common symptom sets + 4 emergency scenarios |
| **Mobile responsive** | Sidebar becomes a slide-in drawer on small screens |

---

## Setup

### 1. Set your backend URL

Open `index.html` and update line ~368:

```js
const API_BASE = 'https://YOUR-APP.onrender.com/api';
```

### 2. Deploy to Vercel

**Option A — Vercel CLI:**
```bash
npm i -g vercel
vercel          # preview
vercel --prod   # production
```

**Option B — GitHub integration:**
Push this repo to GitHub → connect to Vercel dashboard → auto-deploys on every push.

### 3. Update CORS on backend

After deploying, copy your Vercel URL and set it as `CORS_ALLOWED_ORIGINS`
in your Render environment variables.

---

## Local development

```bash
# Any static server works:
npx serve .
python3 -m http.server 3000
```

Point `API_BASE` at `http://localhost:8000/api` for local backend.

---

## File structure

```
medbot-frontend/
├── index.html    ← entire app: HTML + CSS + JS (single file)
├── vercel.json   ← Vercel static deployment config
└── README.md
```

## API contract

| Endpoint | Method | Payload | Response |
|---|---|---|---|
| `/api/chat/` | POST | `{ message, session_id }` | `{ type, message, symptoms_detected, session_id }` |
| `/api/hospitals/` | POST | `{ latitude, longitude, session_id, emergency_id }` | `{ hospitals[], user_location }` |
| `/api/feedback/` | POST | `{ session_id, disease, rating, feedback }` | `{ status, feedback_id }` |
| `/api/health/` | GET | — | `{ status: "ok" }` |
