# 🎓 Batch Wise GPA Calculator

A lightweight web app that lets faculty/admin staff select a **semester and year**, browse the courses offered that term, pull up the enrolled students for a course, and instantly calculate each student's **grade and GPA** from their aggregated marks.

Built with a simple **Express + Neon (Postgres) + Alpine.js** stack no heavy frontend framework, no build step.

---

## ✨ Features

- 📅 **Batch selection** — filter by semester (Fall / Spring / Summer) and year
- 📚 **Dynamic course listing** — pulls distinct courses taught in the selected batch directly from the database
- 👨‍🎓 **Student roster lookup** — view all students enrolled in a course along with their cumulative marks (Quiz + Mid + Final, summed via SQL)
- 🧮 **Instant GPA calculation** — one click converts a student's total marks into a letter grade and GPA using a standard grading scale
- ⚡ **Zero build tooling** — Alpine.js served from CDN, plain HTML/CSS, and a minimal Express backend

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Backend | Node.js, Express |
| Database | PostgreSQL (via [Neon Serverless](https://neon.tech)) |
| Frontend | HTML, Alpine.js |
| Runtime | ES Modules (`type: module`) |

---

## 📂 Project Structure

```
.
├── index.js              # Express app entry point
├── routes/
│   └── batchRoutes.js    # API routes: /api/get-courses, /api/get-students
├── utils/
│   └── db.js             # Neon SQL client (not included — see setup)
├── public/
│   └── app.js             # Alpine.js frontend logic
├── views/
│   └── index.html          # Main UI
├── package.json
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites
- Node.js (v18+)
- A [Neon](https://neon.tech) Postgres database (or any Postgres instance)

### Installation

```bash
git clone https://github.com/<your-username>/batch-wise-gpa-calculation.git
cd batch-wise-gpa-calculation
npm install
```

### Environment Setup

Create a `utils/db.js` file that exports your Neon SQL client:

```js
import { neon } from "@neondatabase/serverless";
export const SQL = neon(process.env.DATABASE_URL);
```

Add a `.env` file (or set the environment variable) with your connection string:

```
DATABASE_URL=postgresql://<user>:<password>@<host>/<db>?sslmode=require
```

### Database Schema

The app expects the following tables (adjust names/columns as needed):
- `recap` — links a course offering (`rid`) to a semester/year (`semester`, `year`, `cid`)
- `course` — course catalog (`cid`, `code`, `title`)
- `student` — student records (`regno`, `name`)
- `marks` — marks entries per student per offering (`regno`, `rid`, `marks`)

### Run the app

```bash
npm start
```

The app will be available at **http://localhost:3000**

---

## 📖 How It Works

1. Select a **semester** and **year**, then click **Show Courses**
2. Click any course to load its **enrolled students** and their cumulative marks
3. Click **Calculate GPA** next to a student to generate their **grade report card**

Grades are calculated using the following scale:

| Marks | Grade | GPA |
|---|---|---|
| 85+ | A | 4.00 |
| 80–84 | A- | 3.66 |
| 75–79 | B+ | 3.33 |
| 71–74 | B | 3.00 |
| 68–70 | B- | 2.66 |
| 64–67 | C+ | 2.33 |
| 61–63 | C | 2.00 |
| 58–60 | C- | 1.66 |
| 54–57 | D+ | 1.30 |
| 50–53 | D | 1.00 |
| < 50 | F | 0.00 |

---

## 🗺️ Roadmap / Ideas for Contribution

- [ ] Batch GPA export (CSV/PDF) for an entire course
- [ ] Class average & grade distribution charts
- [ ] Authentication for faculty access
- [ ] Support for weighted grading components (custom Quiz/Mid/Final weightage)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

## 🙌 Acknowledgements

Built with [Express](https://expressjs.com/), [Alpine.js](https://alpinejs.dev/), and [Neon](https://neon.tech).
