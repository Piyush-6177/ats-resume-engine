# 🚀 ATS Resume Engine

> **AI-Powered Precision. Interview-Ready Confidence.**  
> A full-stack generative AI platform combining automated ATS resume tailoring, intelligent job description match scoring, and personalized interview preparation roadmaps.

[![Node.js](https://img.shields.io/badge/Node.js-18+-339933?style=flat-square&logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![Express.js](https://img.shields.io/badge/Express-5.x-000000?style=flat-square&logo=express&logoColor=white)](https://expressjs.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-Mongoose-47A248?style=flat-square&logo=mongodb&logoColor=white)](https://mongoosejs.com/)
[![Google Gemini](https://img.shields.io/badge/AI-Google_Gemini-8E75C2?style=flat-square&logo=googlegemini&logoColor=white)](https://ai.google.dev/)
[![Puppeteer](https://img.shields.io/badge/PDF-Puppeteer-40B5A4?style=flat-square&logo=puppeteer&logoColor=white)](https://pptr.dev/)
[![React](https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react&logoColor=black)](https://react.dev/)
[![Vite](https://img.shields.io/badge/Vite-7.x-646CFF?style=flat-square&logo=vite&logoColor=white)](https://vitejs.dev/)
[![License: ISC](https://img.shields.io/badge/License-ISC-blue.svg?style=flat-square)](https://opensource.org/licenses/ISC)

---

## ✨ Features

- **🎯 AI Profile Match Scoring**: In-depth comparison between candidate background and job descriptions with a 0–100 relevance score.
- **🔍 Skill Gap Analysis**: Identifies missing or underrepresented competencies categorized by severity (`low`, `medium`, `high`).
- **💡 Smart Interview Preparation**: Generates technical and behavioral interview questions tailored to the position, detailing the interviewer's hidden intentions and winning answers.
- **📅 Step-by-Step Preparation Roadmap**: Generates day-wise action plans with focused practice sessions and study goals.
- **📄 Dynamic ATS Resume Generation**: Produces tailored, ATS-compliant resumes rendered directly into print-ready A4 PDFs via Puppeteer.
- **🔒 Secure Authentication**: Robust JWT cookie-based session management and MongoDB token blacklisting.

---

## 🛠️ Tech Stack

| Layer | Technologies |
| :--- | :--- |
| **Backend API** | Node.js, Express.js (v5), Mongoose (v9), MongoDB |
| **AI & LLM Engine** | Google Gemini API (`@google/genai`), `gemini-3-flash-preview`, Zod |
| **Document Processing**| Puppeteer (Headless Browser), `pdf-parse`, Multer |
| **Auth & Security** | JWT (`jsonwebtoken`), `bcryptjs`, `cookie-parser`, CORS |
| **Frontend Client** | React 19, Vite, React Router v7, Sass / SCSS, Axios |

---

## 📂 Repository Structure

```plaintext
ats-resume-engine/
├── backend/                  # RESTful Express & GenAI API
│   ├── src/
│   │   ├── config/           # Database configuration
│   │   ├── controllers/      # Auth & Interview controllers
│   │   ├── middlewares/      # Auth guard & Multer file handler
│   │   ├── models/           # Mongoose schemas (User, InterviewReport, Blacklist)
│   │   ├── routes/           # Auth & Interview route declarations
│   │   ├── services/         # Gemini GenAI & Puppeteer PDF compiler
│   │   └── app.js            # Express app setup & middleware
│   ├── server.js             # HTTP server entry point (Port 3000)
│   ├── package.json
│   ├── .gitignore
│   ├── .env.example          # Environment variable template
│   └── README.md             # Dedicated backend documentation
│
└── frontend/                 # React Single Page Application (Coming soon)
    ├── src/
    │   ├── features/         # Modular auth & interview modules
    │   │   ├── auth/         # Login, Register, Protected routes, AuthContext
    │   │   └── interview/    # Report dashboard, PDF downloader, InterviewContext
    │   ├── app.routes.jsx    # React Router definitions
    │   └── App.jsx
    ├── index.html
    └── package.json
```

---

## 🚀 Quick Start

### 1. Backend Setup

```bash
cd backend
npm install
```

Configure environment variables:
```bash
cp .env.example .env
```
Fill in `.env`:
```env
PORT=3000
MONGO_URI=mongodb://localhost:27017/gen_resume_helper
JWT_SECRET=your_jwt_secret_key_here
GOOGLE_GENAI_API_KEY=your_google_gemini_api_key_here
```

Start the API server:
```bash
npm run dev
```

The backend runs at `http://localhost:3000`.

### 2. Frontend Setup *(Optional)*

```bash
cd ../frontend
npm install
npm run dev
```

The frontend client runs at `http://localhost:5173`.

---

## 📡 Core API Endpoints

| Method | Route | Access | Function |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/auth/register` | Public | Register new user account |
| `POST` | `/api/auth/login` | Public | Authenticate user & issue JWT cookie |
| `GET` | `/api/auth/logout` | Public | Clear JWT cookie & blacklist token |
| `GET` | `/api/auth/get-me` | Private | Fetch logged-in user profile |
| `POST` | `/api/interview/` | Private | Upload resume & JD to generate AI interview report |
| `GET` | `/api/interview/` | Private | List all reports for the user |
| `GET` | `/api/interview/report/:interviewId` | Private | Get complete interview report details |
| `POST` | `/api/interview/resume/pdf/:interviewReportId` | Private | Generate & stream tailored resume PDF |

---

## 📄 License

Distributed under the [ISC License](LICENSE).
