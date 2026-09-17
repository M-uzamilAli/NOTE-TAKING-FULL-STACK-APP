# 📝 NotesFlow — A Full-Stack Notes App (Learning Project)

A simple notes app I built to actually *do* the full stack myself, end to end — a FastAPI + SQLAlchemy backend talking to a Postgres database on Supabase, a plain HTML/CSS/JS frontend, and a real deployment (Render + GitHub Pages) instead of just running on `localhost`.

> This isn't meant to be a polished product — it's a project I used to learn how the pieces of a full-stack app actually fit together: API design, a real hosted database, CORS, and keeping a free-tier deployment alive. I'm still learning, and this README is as much a log of that as it is documentation.

---

## 🌐 Live Demo

- **UI**: [note-taking-full-stack-app on GitHub Pages](https://m-uzamilali.github.io/NOTE-TAKING-FULL-STACK-APP/)
- **API**: hosted on Render (see `logic.js` for the live endpoint)

> Free-tier hosting spins down when idle, so the first request after a while may take a few seconds to wake up — more on that below.

## 📸 Screenshots

<div align="center">
  <img src="imgs/1.png" width="30%"/>
  <img src="imgs/2.png" width="30%"/>
  <img src="imgs/3.png" width="30%"/>
</div>

---

## 🔥 What It Does

- Register and log in with a username/password
- Create, view, edit (inline title edit + full edit form), and delete notes
- Each user only sees their own notes
- Toast-style notifications for feedback, plus a live online/offline connection indicator

## 🧰 Tech Stack

| Layer      | Technology                                   |
|------------|-----------------------------------------------|
| Frontend   | Plain HTML, CSS, JavaScript (no framework)     |
| Backend    | FastAPI + SQLAlchemy ORM                       |
| Database   | PostgreSQL, hosted on Supabase                 |
| Deployment | Backend on Render, frontend on GitHub Pages    |
| DevOps     | A GitHub Actions cron job to keep the free-tier backend and database from sleeping |

---

## 🧠 Why I Built It This Way (and what I learned)

This was intentionally scoped as a **small, complete** project rather than a big one — I wanted to get through the *whole* loop (frontend → API → database → deployment) rather than getting stuck perfecting one layer. A few specific things I learned along the way:

- **Connecting a FastAPI backend to a real hosted Postgres DB** (via SQLAlchemy) instead of SQLite on my machine, and dealing with connection strings, pooling, and environment-based secrets (`DB-KEY`) for the first time.
- **CORS**, and why a frontend on GitHub Pages calling an API on a different domain needs it configured correctly.
- **Free-tier deployment quirks** — Render's backend and Supabase's database both idle/pause after inactivity, so I set up a small GitHub Actions workflow (`.github/workflows/keep_alive.yml`) that pings both on a schedule to keep them warm. Small thing, but it taught me more about how hosted infra actually behaves than any tutorial did.
- **Designing a minimal REST API** — `/register`, `/login`, `/addNote`, `/getAllNotes`, `/notes/{id}`, `/updateNote/{id}`, `/deleteNote/{id}` — and keeping the request/response shapes simple and consistent.

## 📌 Honest Limitations

Being upfront about where this is still rough, rather than dressing it up:

- **Passwords are stored in plain text** in the database and compared directly — there's no hashing yet. This is the first thing I'd fix if I kept building this out (`passlib`/`bcrypt`, like I've since used in later projects).
- **No real session/auth tokens** — login just checks credentials and the frontend remembers the username in `localStorage`. There's nothing stopping someone from editing that value.
- **No input length/validation feedback from the backend** beyond what the DB column limits enforce.
- **No automated tests.**

I'm listing these because I'd rather be clear about what this project *is* — an early, working full-stack app I learned a lot from — than present it as more finished than it is.

## 🚀 Running It Locally

```bash
# Backend
pip install -r requirements.txt
export DB-KEY=your_supabase_db_password     # or set it in a .env file
uvicorn backend.main:app --reload

# Frontend
# Just open index.html in a browser, or serve the folder with any static server.
# Update the API base URL in logic.js/index.html if you're not using the hosted Render backend.
```

You'll need your own Postgres/Supabase instance and to point `DATABASE_URL` in `backend/main.py` at it.

---

## 📄 License

_Personal learning project_
