# VidyaLink / CampusLink – Internal Learning and Talent Ecosystem

![VidyaLink Banner](https://images.unsplash.com/photo-1523050854058-8df90110c9f1?auto=format&fit=crop&q=80&w=1200&h=400)

## 📖 Project Overview
**VidyaLink / CampusLink** is a full-stack, offline-capable internal academic learning and student talent ecosystem designed for educational institutions. It bridges the gap between self-paced digital learning, real-time peer tutoring, AI-driven personal recommendations, and project talent recruitment within a unified college platform.

The platform empowers students to watch video lessons, attempt interactive assessments, receive parent SMS updates, get AI tutoring via Gemini, showcase portfolio skills, and form project teams. Simultaneously, it equips teachers with analytics dashboards, content management tools, and project moderation capabilities.

---

## 👥 User Roles & Permissions

The system enforces role-based access control (RBAC):

### 1. Student
* **Dashboard**: Personalized learning statistics, recent lessons, AI-driven topic recommendations, and quick access to modules.
* **Capabilities**: Watch video lessons, complete quizzes, ask the AI Tutor questions, request peer tutoring via BuddyUp, showcase skills/projects in SkillSync, track performance analytics in LearnTracker, and create/apply to project hiring posts.
* **Profile Attributes**: Full Name, Email, Department, Year Level, Skills, Parent Phone Number.

### 2. Teacher / Admin
* **Dashboard**: Active student count, aggregate lesson completion statistics, performance analytics charts, and weak topic alerts.
* **Capabilities**: Post new video lessons with optional quizzes, review student performance by topic/department, approve or reject peer tutoring requests, and moderate project hiring posts before campus publication.
* **Profile Attributes**: Full Name, Email, Department, Subjects Teaching, Years of Experience.

---

## 🧩 Core Modules (The 6 Pillars)

### 1. VideoLearner 📚
* Video-on-demand learning module.
* Browse topics, stream embedded lesson videos, read comprehensive text explanations, and access curated external YouTube search resources.
* Automatic local caching for offline viewing.

### 2. TestArena 🎯
* Interactive assessment center.
* Practice quizzes with instant scoring, right/wrong feedback, score counters, and options to retake or ask the AI Tutor for clarification.
* Triggers automated parent SMS alerts upon quiz completion via Twilio.

### 3. BuddyUp 🤝
* Peer-to-peer tutoring and collaboration network.
* Students post help requests for topics they struggle with.
* Other students or teachers can accept requests, offer help, and engage in threaded comment discussions.

### 4. SkillSync 💼
* Internal college talent directory and portfolio showcase.
* Students display technical skills with proficiency levels (Beginner, Intermediate, Advanced) and link showcase projects (GitHub/live URLs).

### 5. LearnTracker 📈
* Real-time analytics dashboard powered by Recharts.
* Visualizes accuracy scores by topic, average progress, and highlights detected weak areas needing review.

### 6. Project Hiring Board 🚀
* Internal project and hackathon recruitment platform.
* Students create hiring posts with required skills, team size, and deadlines.
* **Moderation Pipeline**: Teacher approval required before posts are visible to the student body. Once approved, real-time Socket.io alerts and bulk emails are dispatched.

---

## 🧠 AI & Automated Communication System

* **AI Learning Assistant & Recommendations (Gemini / OpenRouter API)**:
  * **AI Tutor Chat (`AITutorChat`)**: Real-time Q&A assistant powered by `google/gemini-2.5-flash` with speech-to-text input support.
  * **AI Recommendation Engine**: Analyzes student quiz performance history directly from SQLite and generates customized study recommendations and practice questions.
* **Real-Time In-App Notifications (Socket.io)**:
  * Socket.io rooms per user (`user_${userId}`) emit instant updates for post approvals, application status changes, and peer comments directly to the navigation bar.
* **Email Notifications (Nodemailer - Gmail SMTP)**:
  * Sends 6-digit OTP verification codes during registration and password reset.
  * Dispatches bulk emails to all students when teachers post new lessons or approve project hiring opportunities.
* **SMS Parent Alerts (Twilio API)**:
  * Sends text messages to parents (`parent_phone`) when students finish quizzes, reporting their performance.

---

## 📴 Offline-First & Automatic Sync Capabilities

* **Offline Lesson & Quiz Caching**: Lessons and quizzes are saved to browser `localStorage` (`cachedLessons`, `cachedQuizzes_${id}`).
* **Offline Quiz Results Queue**: If an internet connection is lost during a quiz attempt, results are saved locally via `saveOfflineResult()`.
* **Background Auto-Sync (`offlineSync.ts`)**: Registers online event listeners and a 60-second heartbeat. When network connectivity resumes, queued results are synced to `/api/sync`, where SQLite processes them inside an atomic database transaction.

---

## 💻 Technology Stack

* **Frontend**: React 19, Vite 6, Tailwind CSS v4, Lucide React Icons, Recharts, React Router 7.
* **Backend**: Node.js, Express 4, Socket.io 4, `tsx`.
* **Database**: SQLite via `better-sqlite3`.
* **Integrations**: OpenRouter (`google/gemini-2.5-flash`), Nodemailer (Gmail SMTP), Twilio SMS.

---

## 🗄️ Database Schema Overview

The relational SQLite database (`campuslink.db`) consists of 12 tables:
* `users`: Account profiles & role credentials.
* `lessons`: Educational content metadata & video links.
* `quizzes` & `quiz_results`: Questions, options (JSON), attempt accuracy, and timestamps.
* `sync_logs`: Audit log for offline data synchronization.
* `skills` & `projects`: Student portfolio items for SkillSync.
* `peer_requests` & `peer_request_comments`: BuddyUp help requests and discussion threads.
* `hiring_posts` & `hiring_applications`: Project recruitment listings, applicant details, and status.
* `notifications`: User-specific in-app notification state (`is_read`).

---

## 🚀 Setup & Installation Instructions

### 1. Prerequisites
* **Node.js**: v18.0.0 or higher
* **npm**: v9.0.0 or higher

### 2. Installation
Navigate into the application directory and install dependencies:
```bash
cd Campuslink-main
npm install
```

### 3. Environment Configuration
Create a `.env` file in `Campuslink-main/`:
```env
# AI Keys
VITE_OPENROUTER_API_KEY=your_openrouter_api_key
GEMINI_API_KEY=your_gemini_api_key

# Email Settings (Nodemailer Gmail SMTP App Password)
SMTP_PASS=your_gmail_app_password

# Twilio SMS Credentials (Optional)
TWILIO_ACCOUNT_SID=ACXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
TWILIO_AUTH_TOKEN=your_auth_token
TWILIO_PHONE_NUMBER=+1234567890

# App Config
APP_URL=http://localhost:3000
```

### 4. Running the Development Server
```bash
npm run dev
```
Access the application at `http://localhost:3000`.

### 5. Demo Accounts
The SQLite database automatically seeds on first launch:
* **Student**: `student@campuslink.com` / `password`
* **Teacher**: `teacher@campuslink.com` / `password`

---

## Developed By

**Abirubha P**

---

