# 📄 ATS Resume Engine — Backend API

[![Node.js](https://img.shields.io/badge/Node.js-18+-339933?style=flat-square&logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![Express.js](https://img.shields.io/badge/Express-5.x-000000?style=flat-square&logo=express&logoColor=white)](https://expressjs.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-Mongoose-47A248?style=flat-square&logo=mongodb&logoColor=white)](https://mongoosejs.com/)
[![Google Gemini](https://img.shields.io/badge/AI-Google_Gemini-8E75C2?style=flat-square&logo=googlegemini&logoColor=white)](https://ai.google.dev/)
[![Puppeteer](https://img.shields.io/badge/PDF-Puppeteer-40B5A4?style=flat-square&logo=puppeteer&logoColor=white)](https://pptr.dev/)
[![License: ISC](https://img.shields.io/badge/License-ISC-blue.svg?style=flat-square)](https://opensource.org/licenses/ISC)

A scalable, intelligent RESTful API backend for ATS resume optimization and interview preparation. Built with **Node.js**, **Express 5**, **MongoDB**, **Google Gemini GenAI**, and **Puppeteer**, providing candidate evaluation, automated interview prep roadmaps, and dynamic ATS-friendly PDF resume compilation.

---

## 🌟 Key Features

- **🎯 AI Match Scoring & Gap Analysis**:
  - Scores candidate resumes against target job descriptions (0–100 match score).
  - Pinpoints specific skill gaps with severity rankings (`low`, `medium`, `high`).
- **💡 Comprehensive Interview Preparation Engine**:
  - Generates role-specific technical and behavioral questions.
  - Explains the interviewer's hidden intention behind each question.
  - Provides model answers and best-practice strategic responses.
  - Curates custom day-by-day preparation plans with action items.
- **📑 Dynamic ATS-Optimized PDF Resumes**:
  - Tailors candidate resumes to target job requirements with human-like phrasing.
  - High-fidelity PDF rendering via headless **Puppeteer**.
- **🔒 Secure Authentication & Token Blacklisting**:
  - Password hashing with `bcryptjs` (10 salt rounds).
  - Stateless JWT token transmission via HTTP cookies.
  - MongoDB-backed token blacklist invalidating sessions on logout.
- **⚡ In-Memory File Processing**:
  - Multer memory storage eliminates disk overhead for uploaded resume PDFs.
  - Real-time text extraction using `pdf-parse`.

---

## 🛠️ Tech Stack

| Layer | Technology |
| :--- | :--- |
| **Runtime & Framework** | Node.js, Express.js (v5) |
| **Database & ODM** | MongoDB, Mongoose (v9) |
| **Generative AI** | Google Gemini API (`@google/genai`), `gemini-3-flash-preview` |
| **Schema Validation** | Zod, Zod-to-JSON-Schema |
| **PDF Processing** | Puppeteer (Headless Browser), `pdf-parse`, Multer |
| **Authentication** | JSON Web Tokens (`jsonwebtoken`), `bcryptjs`, `cookie-parser` |
| **Environment & Config** | Dotenv, CORS |

---

## 📂 Project Architecture

```plaintext
backend/
├── src/
│   ├── config/              # Infrastructure configuration
│   │   └── database.js      # MongoDB connection setup
│   ├── controllers/         # Request handling & controller logic
│   │   ├── auth.controller.js
│   │   └── interview.controller.js
│   ├── middlewares/         # Middleware guards
│   │   ├── auth.middleware.js
│   │   └── file.middleware.js
│   ├── models/              # Mongoose schemas & data models
│   │   ├── blacklist.model.js
│   │   ├── interviewReport.model.js
│   │   └── user.model.js
│   ├── routes/              # Express API route declarations
│   │   ├── auth.routes.js
│   │   └── interview.routes.js
│   ├── services/            # Core business logic & AI orchestration
│   │   └── ai.service.js    # Gemini prompt engineering & Puppeteer PDF compiler
│   └── app.js               # Express application initialization & middleware
├── server.js                # Server entry point (Port 3000)
├── package.json
├── .gitignore
└── .env.example             # Template for required environment variables
```

---

## 🚀 Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) (v18 or higher)
- [MongoDB](https://www.mongodb.com/) (Local instance or Atlas connection string)
- [Google Gemini API Key](https://aistudio.google.com/)

### Installation

1. **Navigate to the backend directory**:
   ```bash
   cd backend
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Configure Environment Variables**:
   Copy the example template to create your `.env` file:
   ```bash
   cp .env.example .env
   ```
   Fill in your configuration:
   ```env
   PORT=3000
   MONGO_URI=mongodb://localhost:27017/gen_resume_helper
   JWT_SECRET=your_super_secret_jwt_key
   GOOGLE_GENAI_API_KEY=your_gemini_api_key_here
   ```

4. **Start the Server**:
   ```bash
   # Development mode (with live reload)
   npm run dev

   # Production mode
   npm start
   ```
   The server will start listening at `http://localhost:3000`.

---

## 📡 API Reference

### 🔐 Authentication Endpoints (`/api/auth`)

| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/auth/register` | Public | Register a new user account. Sets auth cookie. |
| `POST` | `/api/auth/login` | Public | Authenticate user with credentials. Sets auth cookie. |
| `GET` | `/api/auth/logout` | Public | Blacklist active token and clear auth cookie. |
| `GET` | `/api/auth/get-me` | Private | Retrieve authenticated user profile. |

#### Register Request Body:
```json
{
  "username": "johndoe",
  "email": "john@example.com",
  "password": "StrongPassword123"
}
```

---

### 📋 Interview & Resume Endpoints (`/api/interview`)

| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/interview/` | Private | Upload resume (PDF), job description, and generate AI interview report. |
| `GET` | `/api/interview/` | Private | Get list of all interview reports for the logged-in user. |
| `GET` | `/api/interview/report/:interviewId` | Private | Retrieve full details of a specific interview report. |
| `POST` | `/api/interview/resume/pdf/:interviewReportId` | Private | Generate and download an ATS-optimized PDF resume. |

#### Generate Report (`multipart/form-data`):
- `resume`: Candidate Resume (`.pdf` file, max 3MB)
- `jobDescription`: Target job description text (string)
- `selfDescription`: Candidate's personal summary/pitch (string)

---

## 🛡️ Security & Design Decisions

- **Structured AI Outputs**: Prompts are validated with strict **Zod schemas** transformed into JSON schemas via `zod-to-json-schema`, guaranteeing deterministic JSON responses from Gemini.
- **Headless Document Rendering**: Resumes are converted from ATS-compliant HTML templates into print-ready A4 PDFs using headless Puppeteer on-the-fly.
- **In-Memory Buffers**: Uploaded PDFs and generated output documents are buffered in memory via Multer, avoiding ephemeral storage leaks on containerized platforms.
- **Token Blacklist**: Logout invalidates JWT tokens server-side in MongoDB with automated TTL expiration.

---

## 📄 License

This project is licensed under the [ISC License](LICENSE).
