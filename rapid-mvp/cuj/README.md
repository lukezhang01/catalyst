# Rapid MVP - Classroom Presentation Randomizer

This folder contains the Critical User Journey (CUJ) framework, operational reflections, and audit artifacts for Assignment 2.

## Quick Links
* **CUJ Framework & Friction Audit:** [framework.md](./framework.md)
* **Post-Mortem (Done vs. Usable):** [lessons-learned.md](./lessons-learned.md)
* **Team Principles Reflection:** [reflections.md](./reflections.md)

## Persona & Core Use Case
* **Target User:** Prof. John Doe (Course Instructor outside the CS department)
* **Objective:** Randomly select project teams, track presentation and Q&A timing, and manage classroom rosters with minimal operational friction.

## Tech Stack & Architecture
* **Frontend:** React (Vite), JavaScript, Custom Hooks (`useDualTimer.js`), CSS3
* **Backend:** Python (FastAPI / Uvicorn), JSON storage (`store.py`, `session.json`)
* **Architecture:** Fully decoupled Frontend/Backend separation running locally, architected for cloud migration in Assignment 3.

## Local Setup Instructions

> **Note on Dependencies:** This MVP requires pre-installed runtime environments (Python 3.10+ and Node.js v18+). As identified in our CUJ audit, non-technical users must ensure these prerequisites are installed before execution.

### 1. Backend Setup
```bash
cd backend
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

### 2. Fronted Setup
```bash
cd frontend
npm install
npm run dev
```

direct link to code repo: [randomizer code](https://github.com/OliviaY1/randomizer)
