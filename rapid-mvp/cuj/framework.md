# Critical User Journey (CUJ) Framework Document

## 1. User Goal & Persona
* **Persona:** Prof. John Doe, a university course instructor from outside the CS department managing a high-density presentation session with 40+ students.
* **User Goal:** Randomly select project teams, display metadata, track presentation and Q&A time using a dual-phase timer, and manage team rosters without creating classroom friction or losing track of time.

---

## 2. Journey Audit (The Unhappy Path)

| Step | User Action | Interface Response | Context Switch / Mental Gear | Time Spent|
| :--- | :--- | :--- | :--- | :--- |
| **1. Environment Setup** | Tries to launch local app; lacks Python/Node prerequisites as a non-CS prof. | Command line errors (`python/node command not found`). | Major friction searching online guides to build a Python virtual environment (`venv`) and install Node.js/npm. | 20 mins |
| **2. App Launch** | Executes local FastAPI backend and Vite/React frontend scripts. | Dashboard loads in zero state: *"Who's first? Draw a team to begin."* | Mental gear shift from terminal setup to web application interface. | 45s |
| **3. Sequential Roster Setup** | Enters team names, projects, and member names one by one into the sidebar form. | Items are saved to `session.json` and listed under "Waiting". | Severe cognitive and administrative friction manually typing every student name and team during session setup. | 15 mins |
| **4. Drawing a Team** | Clicks the primary **Draw first team** button. | Calls backend randomizer endpoint; hero area updates with selected team metadata. | Low friction; single visual transition. | 2s |
| **5. Presentation Phase** | Triggers the 7-minute Presentation Timer. | Clock counts down; visual warning cue fires at 2:00 remaining. | Instructor splits focus between grading student presentation and watching the timer. | 420s |
| **6. Q&A Transition** | Presentation timer completes; audio chime plays via `chime.js`. | Dual timer shifts automatically to the dedicated Q&A countdown phase. | Manual gear shift to moderate Q&A questions from the audience. | 180s |
| **7. Dynamic Registration** | Adds a late-arriving team and member names manually while active timers run. | Sidebar updates waiting pool instantly via client-side state mutation. | High friction context switch while actively managing presenters and grading. | 45s |
All supporting screenshots could be found in the assets folder and in the presentation slides.

### Quantitative Metrics
* **Total Setup Overhead:** ~35 minutes (20 min runtime environment setup + 15 min manual team-by-team entry).
* **Total Journey Cycle Time:** ~13.5 minutes per presentation/Q&A run.
* **Total Context Switches:** 4 major switches (Browser search/CLI setup $\rightarrow$ Terminal launch $\rightarrow$ Manual Form Entry $\rightarrow$ Live Presentation Moderation).

---

## 3. Analysis & Recommendations

### Highlights & Lowlights

| Severity | Category | Detail |
| :--- | :--- | :--- |
| **Great (Highlight)** | **Timer Guidance** | Dual-phase state transitions (`useDualTimer.js`) combined with visual/audible alerts keep the room focused without manual intervention. |
| **Severe (Lowlight)** | **Environment Barrier** | Requiring local CLI execution (`venv`, `npm install`) creates a massive setup barrier for non-CS instructors like Prof. John Doe. |
| **Severe (Lowlight)** | **Input Bottleneck** | Typing team details and member lists one by one via `AddTeamForm.jsx` wastes valuable class time. |

### Product Recommendations
1. **Containerization / Cloud Single-Click Run:** Dockerize the application or deploy a hosted web endpoint so non-technical instructors do not need to configure local Python or Node environments.
2. **Bulk Roster Import (CSV/JSON):** Upgrade `AddTeamForm.jsx` with a bulk file upload button so instructors can import complete class lists instantly instead of entering teams one by one.

### Future User Advice (Pro-Tips)
* **Pre-install Dependencies:** Ensure Node.js and Python are installed and verified prior to presentation day.
* **Pre-fill Storage:** Manually edit `backend/session.json` ahead of class to populate all teams and members, avoiding form typing during lecture hours.
* **Audio Permissions:** Trigger a test timer on startup to ensure browser autoplay policies do not block `chime.js`.