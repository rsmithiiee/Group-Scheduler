# Group Scheduler

A collaborative scheduling web application that lets users manage personal calendars, form groups, and instantly find shared free time windows — perfect for coordinating group projects, study sessions, and team meetings.

![Next.js](https://img.shields.io/badge/Next.js-14-black?logo=next.js)
![Flask](https://img.shields.io/badge/Flask-Python-blue?logo=flask)
![SQLite](https://img.shields.io/badge/SQLite-3-green?logo=sqlite)
![Tailwind CSS](https://img.shields.io/badge/Tailwind-CSS-38B2AC?logo=tailwind-css)

---

## What It Does

Group Scheduler is a full-stack scheduling tool with two main parts:

- **Personal Calendar** — Create, view, and manage your own events on an interactive calendar
- **Group Free Time Finder** — Create or join groups with other users, then query for windows of time when every group member is free

---

## Features

- User authentication with secure password hashing (Argon2)
- Interactive calendar powered by FullCalendar (month/week/day views)
- Group management — create groups, add/remove members by username
- Smart free-time algorithm that merges overlapping events and returns available slots across all group members
- RESTful Flask API backend with SQLite persistence
- Next.js 14 frontend with Tailwind CSS + Flowbite components

---

## Tech Stack

| Layer     | Technology                              |
|-----------|-----------------------------------------|
| Frontend  | Next.js 14, React 18, TypeScript        |
| Styling   | Tailwind CSS, Flowbite React            |
| Calendar  | FullCalendar 6                          |
| Backend   | Python, Flask, Flask-SQLAlchemy         |
| Database  | SQLite (via SQLAlchemy ORM)             |
| Auth      | Argon2 password hashing                 |

---

## Project Structure

```
CMPE_131_Project/
├── Flask/                         # Backend
│   ├── app.py                     # Flask routes & API endpoints
│   ├── flask_sqlalchemy_db_setup.py  # SQLAlchemy models
│   └── algorithm.py               # Free-time computation logic
├── cmpe_131_project_frontend/     # Frontend (Next.js)
│   └── src/app/
│       ├── page.js                # Landing page
│       ├── (auth)/
│       │   ├── login/             # Login page
│       │   └── create-account/    # Registration page
│       └── [dashboardUser]/       # Per-user dashboard
│           ├── page.tsx           # Dashboard layout
│           ├── calendar-components/  # FullCalendar integration
│           └── sidebar-components/   # Groups & people sidebar
└── Database/                      # SQLite database files
```

---

## Getting Started

### Prerequisites

- **Python** 3.10+
- **Node.js** 18+ and **npm**
- **pip**

---

### 1. Clone the Repository

```bash
git clone https://github.com/<your-org>/Group-Scheduler.git
cd Group-Scheduler
```

---

### 2. Set Up the Backend

```bash
cd Flask

# Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# Install dependencies
pip install flask flask-sqlalchemy flask-cors argon2-cffi

# Start the Flask server
python app.py
```

The API will be available at `http://localhost:5000`.

The SQLite database (`official.db`) is automatically created in `Flask/instance/` on first run.

---

### 3. Set Up the Frontend

```bash
cd cmpe_131_project_frontend

# Install dependencies
npm install

# Start the development server
npm run dev
```

The app will be available at `http://localhost:3000`.

> **Note:** The frontend expects the Flask API at `http://localhost:5000`. Both servers must be running simultaneously.

---

### 4. Using the App

1. Navigate to `http://localhost:3000` and create an account
2. Log in — you'll be taken to your personal dashboard at `/<your-username>`
3. Click any time slot on the calendar to create an event
4. Use the sidebar to create a group or add other registered users by username
5. Select a group and a date range, then click **Find Free Times** to see when everyone is available

---

## API Endpoints

All endpoints accept and return JSON.

| Method | Endpoint                         | Description                              |
|--------|----------------------------------|------------------------------------------|
| POST   | `/api/create_account`            | Register a new user                      |
| POST   | `/api/login`                     | Authenticate a user                      |
| POST   | `/api/retrieve_user_info`        | Get user ID and group memberships        |
| POST   | `/api/retrieve_user_events`      | List events for a user                   |
| POST   | `/api/create_event`              | Add a new calendar event                 |
| POST   | `/api/edit_event`                | Update an existing event                 |
| POST   | `/api/delete_event`              | Remove an event                          |
| POST   | `/api/create_group`              | Create a new group                       |
| POST   | `/api/add_users_group`           | Add a user to a group by username        |
| POST   | `/api/delete_user_group`         | Remove a user from a group               |
| POST   | `/api/retrieve_group_free_times` | Find free windows across all group members |

---

## How the Free-Time Algorithm Works

`Flask/algorithm.py` computes shared availability in four steps:

1. **Parse** — ISO 8601 date strings are converted to `datetime` objects
2. **Sort** — Events are sorted by start time, then end time (insertion sort, optimized for small datasets)
3. **Merge** — Overlapping intervals are collapsed into a single block
4. **Gap detection** — Gaps between consecutive merged blocks are returned as free windows

The result is a JSON array of `{ start, end }` objects representing periods when every group member is free.

---

## Database Schema

| Table         | Key Columns                                              |
|---------------|----------------------------------------------------------|
| `Users`       | `User_ID`, `First_Name`, `Last_Name`, `Username`, `Password` |
| `User_Events` | `Event_ID`, `User_ID` (FK), `Event_Name`, `Start_Time`, `End_Time` |
| `Groups`      | `Group_ID`, `Group_Name`                                 |
| `Groups_Users`| `Group_ID` (FK), `User_ID` (FK) — many-to-many join table |

---

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Commit your changes: `git commit -m "Add your feature"`
4. Push to your fork: `git push origin feature/your-feature-name`
5. Open a Pull Request against `main`

Please make sure the Flask server starts cleanly and the frontend builds without errors (`npm run build`) before submitting.

---

## Maintainers

This project was developed as part of **CMPE 131 — Software Engineering** at San José State University by:

- Ryan Smith
- Hemanth Karnati
- Alex Cheong
- Michael Chan
---

## License
This project is for academic use.
