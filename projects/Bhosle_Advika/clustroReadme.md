# Project Title: Clustro – Task management meets productivity in one place.

## Student Details
- **Name**: Advika Bhosle
- **PRN**: 24070123007
- **Year**: SY  
- **Branch**: E&TC

---

## Problem Statement
Trello helps organize tasks but lacks built-in productivity tools, focus-only modes, analytics, and offline access. This forces users to switch between multiple apps, making task execution inefficient. Clustro addresses this gap by combining task management with focus features, productivity tracking, and offline functionality in one platform.

---

## Features
Task Management – Create, edit, delete, and organize tasks into boards and lists.

Focus Mode (Today’s Tasks Only) – Show only tasks due today to reduce distractions.

Pomodoro Timer – Built-in timer inside tasks for focused work sessions.

Offline Mode – Access and update tasks even without internet, syncs later.

Basic Analytics Dashboard – Track completed tasks, productivity trends, and time spent.

User Authentication – Secure signup/login to manage personal boards.

Collaboration (Optional) – Share boards and assign tasks to team members.

Search & Filter – Quickly find tasks by keywords, deadlines, or tags.

Notifications/Reminders – Get notified about upcoming or overdue tasks.

Simple & Clean UI – User-friendly, minimal interface for smooth navigation.

---

## Tech Stack
1. Frontend (User Interface)

Languages:

HTML5

CSS3 (with Tailwind CSS or Bootstrap for styling)

JavaScript (for interactivity)

Frameworks/Libraries:

React.js (recommended for a responsive and dynamic UI)

2. Backend (Server & API)

Languages:

JavaScript / TypeScript

Frameworks:

Node.js

Express.js (for building REST APIs)

3. Database

Primary: MongoDB (NoSQL, flexible for tasks/boards)

Alternative: PostgreSQL or MySQL (if you prefer SQL-based relational DB)

4. Authentication & Security

JSON Web Tokens (JWT)

bcrypt.js (for password hashing)

5. Productivity Features

Pomodoro Timer: Vanilla JS / React hooks

Analytics Dashboard: Chart.js / Recharts

6. Version Control & Collaboration

Git & GitHub (for repository management, branching, pulling)

7. Deployment / Hosting

Frontend: Vercel / Netlify

Backend: Render / Railway / Heroku

Database: MongoDB Atlas

8. Developer Tools

VS Code (IDE)

Postman (API testing)

GitHub Copilot (for coding assistance)

---

## How to Run
Prerequisites (install once)

Install Python 3.8+, Node (npm), and MySQL (or run a MySQL Docker container).

Have git and VS Code installed.

Open the project in VS Code: File → Open Folder → <repo root>.

Quick overview of what we’ll run

Backend (FastAPI + SQLAlchemy) — serves API on http://127.0.0.1:8000

Frontend (Vite + React + Tailwind) — runs on http://localhost:5173
 (or the port Vite shows)

Database: MySQL database clustro_db (you create it once)

A. Prepare the database (MySQL)

Start MySQL server (or Docker container).

Create the database (run in terminal where mysql is available):

PowerShell / mac/linux:

# using root (it will prompt for root password)
mysql -u root -p -e "CREATE DATABASE IF NOT EXISTS clustro_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"


If you want a dedicated DB user (recommended):

CREATE USER 'clustro_user'@'localhost' IDENTIFIED BY 'YourStrongPass';
GRANT ALL PRIVILEGES ON clustro_db.* TO 'clustro_user'@'localhost';
FLUSH PRIVILEGES;


(You can run the above SQL via mysql -u root -p and paste the commands.)

Note: If you use Docker, run MySQL container and then docker exec to run the CREATE DATABASE command.

B. Backend — FastAPI (Windows PowerShell steps)

Open a new PowerShell terminal in VS Code, then:

cd to backend:

cd .\projects\bhosle_advika\backend_fastapi


Create & activate virtual environment:

python -m venv venv
# Activate (PowerShell)
.\venv\Scripts\Activate.ps1


(macOS / Linux)

python3 -m venv venv
source venv/bin/activate


Install Python dependencies:

pip install -r requirements.txt


Create .env from example and edit with your credentials:

copy .env.example .env
# Then open .env in editor and set DB_PASS, JWT_SECRET, etc.
code .env   # if using VS Code to edit


In .env set DB_USER, DB_PASS, DB_HOST, DB_NAME, JWT_SECRET.

(Optional) Ensure the DB exists (see A). SQLAlchemy will create tables automatically the first time backend runs.

Start the backend server:

uvicorn main:app --reload


You should see Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit).

Confirm API is up: open http://127.0.0.1:8000/docs in your browser to see the Swagger UI.

C. Frontend — Vite + React (PowerShell steps)

Open a second terminal in VS Code:

cd to frontend:

cd .\projects\bhosle_advika\frontend


Install Node dependencies:

npm install


Create frontend env (so React knows backend URL):

# Windows PowerShell: create .env file with API base
set-content -path .env -value "VITE_API=http://127.0.0.1:8000"
# or open .env in editor and paste: VITE_API=http://127.0.0.1:8000
code .env


(Unix)

echo "VITE_API=http://127.0.0.1:8000" > .env


Start the frontend dev server:

npm run dev


Vite will show a URL (usually http://localhost:5173) — open that in your browser.

D. Test the app (end-to-end checklist)

Open frontend UI (http://localhost:5173).

Click Sign up and create a new user (username & password).

Login with that user. After login the app stores the JWT in localStorage (key clustro_token) and clustro_user (user_id).

Create a board (+ Board). Default lists ("To Do", "In Progress", "Done") are seeded automatically.

Add a new card in any list using Quick Add (optionally set due date).

Drag & drop cards between lists — changes are persisted to backend (PATCH /cards/{id}/move).

Toggle Focus (today) checkbox — it should show only cards due today (calls /focus/{user_id}).

Try the Pomodoro timer in a card — Start/Pause/Reset should work locally.

If everything behaves, backend and frontend are correctly connected.

E. Run both with simple script (optional)

You can create a quick PowerShell script run-all.ps1 at projects\bhosle_advika/ to start backend and frontend (dev). Paste this:

run-all.ps1

# run-all.ps1 (PowerShell)
Start-Process -NoNewWindow -FilePath "powershell" -ArgumentList "-NoExit", "-Command", "cd ./projects/bhosle_advika/backend_fastapi; .\venv\Scripts\Activate.ps1; uvicorn main:app --reload"
Start-Sleep -Seconds 2
Start-Process -NoNewWindow -FilePath "powershell" -ArgumentList "-NoExit", "-Command", "cd ./projects/bhosle_advika/frontend; npm run dev"


Run it:

.\run-all.ps1


(Each will open in its own PowerShell window.)

For macOS/Linux you can write a simple run-all.sh that runs backend in background and then npm run dev.

F. Troubleshooting (common issues)

DB connection error: verify .env values, MySQL running, and the DB exists. Try connecting with mysql -u DB_USER -p.

Port in use: if 8000 or 5173 are busy, close processes or change ports (uvicorn: --port 8001, Vite: --port 3000 via npm run dev -- --port 3000).

CORS issues: backend already sets allow_origins=["*"] for dev. If you changed it, add http://localhost:5173.

Token decode / focus not working: confirm localStorage.clustro_user holds user_id (set during login).

Python module errors: ensure venv activated and pip install -r requirements.txt completed.

React build errors: run npm install again, remove node_modules and reinstall if needed.

G. Quick verification commands

See running uvicorn logs: look at the backend terminal for requests when you interact with frontend.

Check network requests in browser DevTools → Network to confirm calls to http://127.0.0.1:8000.

H. Stop servers

Backend: press Ctrl + C in uvicorn terminal.

Frontend: press Ctrl + C in Vite terminal.
---

## Project Structure
Project Structure
clustro/
│
├── backend/
│   ├── server.js
│   ├── package.json
│   └── routes/
│       └── tasks.js
│
├── frontend/
│   ├── index.html
│   ├── style.css
│   └── app.js
│
└── README.md

x
▶️ How to Run

Clone repo & open in VS Code

git clone https://github.com/AdvikaGitHub12/Bhosle_Advika.git
cd Bhosle_Advika


Backend Setup

cd backend
npm install
npm start


(Server runs at http://localhost:5000
)

Frontend Setup

Open frontend/index.html in your browser (just double-click OR use VS Code Live Server extension).

Git Upload

git add .
git commit -m "Clustro initial project"
git push origin main

---

## Demo Screenshot / Output
![WhatsApp Image 2025-08-26 at 23 09 45_133da0c6](https://github.com/user-attachments/assets/e150ec94-f688-452b-bef0-7a1d70867d6f)


---

## AI Tools Used
For the Clustro project, the AI tools used are:

ChatGPT – for generating backend and frontend code templates, project structure, explanations, and setup guidance.

GitHub Copilot – for autocompletion, boilerplate generation, and code suggestions inside VS Code.
---

## Future Improvements
Enhanced Features: I would add features like personalized content recommendations, social interactions between users, and advanced search filters to make Clustro more engaging and user-friendly.

Better UI/UX Design: I would improve the design to be more modern, responsive, and mobile-friendly, ensuring smooth navigation and an intuitive experience.

API Integration: I would integrate external APIs for real-time data, such as social media logins, trending content feeds, or analytics services to make Clustro more dynamic.

Database Optimization: I would enhance data management to allow faster searches, better scalability, and efficient handling of user data.

Security Enhancements: I would implement stronger authentication, secure data storage, and encryption to protect user information.
---

## Notes for Reviewers
Any extra note for the FOSS team.  
Example: "This project runs offline by default." or "Needs an internet connection."

---

## Submission Checklist 
- [x] Cloned the Repository 
- [x] Added my details (Name, PRN, Year, Branch)  
- [x] Wrote Problem Statement  
- [x] Listed Features & Tech Stack  
- [x] Added clear Run Instructions  
- [x] Provided Demo Output (screenshot or text)  
- [x] Listed AI tools used (or None)  
- [x] Explained Future Improvements  
- [ ] Project runs offline

