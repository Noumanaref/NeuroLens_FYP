# 🧠 NeuroLens — Real-Time Emotion-Aware Wellbeing System

<p align="center">
  <strong>An AI-powered desktop application that monitors facial emotions and screen activity in real-time to promote mental well-being through personalized recommendations.</strong>
</p>

---

## 📖 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Setup & Installation](#setup--installation)
  - [1. Clone the Repository](#1-clone-the-repository)
  - [2. Backend Setup (FastAPI)](#2-backend-setup-fastapi)
  - [3. Frontend Setup (Flutter)](#3-frontend-setup-flutter)
  - [4. Environment Variables](#4-environment-variables)
  - [5. Database Setup](#5-database-setup)
- [Running the Application](#running-the-application)
- [Features](#features)
- [API Endpoints](#api-endpoints)
- [Contributors](#contributors)

---

## Overview

**NeuroLens** is a Final Year Project (FYP) that combines facial emotion detection, screen activity classification, and behavioral analysis to create a holistic mental well-being monitoring system. The system captures the user's face via webcam, detects emotions in real-time using deep learning models, classifies on-screen activity, and delivers personalized content recommendations when prolonged negative emotional states are detected.

---

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Flutter Frontend                      │
│  (Camera Capture → API Calls → Dashboard & Reports)      │
└──────────────────────┬──────────────────────────────────┘
                       │  HTTP / REST
                       ▼
┌─────────────────────────────────────────────────────────┐
│                  FastAPI Backend (main.py)                │
│  ┌──────────────┐  ┌──────────────┐  ┌───────────────┐  │
│  │ Auth & Users  │  │ Emotion API  │  │ Recommendations│  │
│  │ (JWT + Argon2)│  │ (DeepFace)   │  │ Engine        │  │
│  └──────────────┘  └──────────────┘  └───────────────┘  │
│  ┌──────────────┐  ┌──────────────┐  ┌───────────────┐  │
│  │ Email Service │  │ Encryption   │  │ Reports &     │  │
│  │ (SMTP/Gmail)  │  │ (Fernet)     │  │ Analytics     │  │
│  └──────────────┘  └──────────────┘  └───────────────┘  │
└──────────────────────┬──────────────────────────────────┘
                       │
          ┌────────────┴────────────┐
          ▼                         ▼
┌──────────────────┐    ┌──────────────────────────┐
│  PostgreSQL DB   │    │   Activity Module        │
│  (User data,     │    │   (CLIP + Florence-2 +   │
│   Emotion logs,  │    │    Qwen — Screen Activity │
│   Sessions)      │    │    Classification)        │
└──────────────────┘    └──────────────────────────┘
```

---

## Tech Stack

| Layer         | Technology                                                  |
| ------------- | ----------------------------------------------------------- |
| **Frontend**  | Flutter (Dart) — Cross-platform (Web, Windows, Android)     |
| **Backend**   | FastAPI (Python) — REST API with async support               |
| **Database**  | PostgreSQL — Relational data storage with SQLAlchemy ORM     |
| **Emotion AI**| DeepFace — Real-time facial emotion recognition              |
| **Activity AI**| CLIP + Florence-2 + Qwen — Screen content classification   |
| **Auth**      | JWT tokens + Argon2 password hashing                         |
| **Encryption**| Fernet symmetric encryption for PII data                     |
| **Email**     | Gmail SMTP with App Passwords                                |

---

## Project Structure

```
NeuroLens/
├── main.py                  # FastAPI backend (API routes, auth, emotion analysis)
├── config.py                # Application settings (loaded from .env)
├── database.py              # SQLAlchemy database engine & session
├── models.py                # Database models (User, EmotionLog, etc.)
├── schemas.py               # Pydantic request/response schemas
├── auth.py                  # JWT authentication helpers
├── encryption.py            # Fernet encryption service for PII
├── email_service.py         # Gmail SMTP email service
├── emotion_model.py         # DeepFace emotion detection wrapper
├── window_detector.py       # Active window detection & screen capture
├── terms_and_conditions.py  # Legal text content
├── create_admin.py          # Script to create admin user
├── requirements.txt         # Python dependencies
├── .env                     # Environment variables (not committed)
│
├── Activity_Module/         # Screen activity classification pipeline
│   ├── main.py              # Activity module FastAPI server
│   ├── router.py            # API routes for activity classification
│   ├── config.py            # Activity module configuration
│   ├── client_runner.py     # Screen capture & inference client
│   ├── activity_classifier/ # CLIP + Florence-2 + Qwen classifiers
│   └── requirements.txt     # Activity module dependencies
│
└── neurolens_front/         # Flutter frontend application
    ├── lib/
    │   ├── main.dart        # App entry point
    │   ├── models/          # Data models (EmotionModel, ContentModel, etc.)
    │   ├── providers/       # State management (ChangeNotifier providers)
    │   ├── screens/         # UI screens (Login, Dashboard, Camera, Reports, etc.)
    │   ├── services/        # API service, notification service
    │   ├── utils/           # Utility helpers
    │   └── widgets/         # Reusable UI components
    ├── assets/              # App assets (logo, strings)
    └── pubspec.yaml         # Flutter dependencies
```

---

## Prerequisites

Before running NeuroLens, ensure you have the following installed:

| Tool            | Version     | Download Link                                                |
| --------------- | ----------- | ------------------------------------------------------------ |
| **Python**      | 3.10+       | [python.org](https://www.python.org/downloads/)               |
| **Flutter SDK** | 3.10+       | [flutter.dev](https://docs.flutter.dev/get-started/install)   |
| **PostgreSQL**  | 14+         | [postgresql.org](https://www.postgresql.org/download/)        |
| **Git**         | Latest      | [git-scm.com](https://git-scm.com/downloads)                 |
| **Chrome**      | Latest      | Required for Flutter web builds                               |

> **Optional:** Install [Visual Studio 2022](https://visualstudio.microsoft.com/downloads/) with "Desktop development with C++" workload to build Windows desktop apps.

---

## Setup & Installation

### 1. Clone the Repository

```bash
git clone https://github.com/Noumanaref/NeuroLens_FYP.git
cd NeuroLens_FYP
```

---

### 2. Backend Setup (FastAPI)

```bash
# Create a virtual environment
python -m venv venv

# Activate it
# Windows (PowerShell):
.\venv\Scripts\Activate
# Windows (CMD):
venv\Scripts\activate.bat
# macOS/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

---

### 3. Frontend Setup (Flutter)

```bash
cd neurolens_front

# Get Flutter dependencies
flutter pub get

# Verify your setup
flutter doctor
```

---

### 4. Environment Variables

Create a `.env` file in the **project root** (`NeuroLens_FYP/`) with the following:

```env
DATABASE_URL="postgresql+psycopg2://postgres:YOUR_PASSWORD@localhost:5432/neurolens_db"
SECRET_KEY="your-secret-key-here"
ENCRYPTION_KEY="your-fernet-encryption-key"
EMAIL_USER="your-email@gmail.com"
EMAIL_PASSWORD="your-gmail-app-password"
```

#### Generate Keys

```python
# Generate SECRET_KEY
python -c "import secrets; print(secrets.token_hex(32))"

# Generate ENCRYPTION_KEY (Fernet)
python -c "from cryptography.fernet import Fernet; print(Fernet.generate_key().decode())"
```

#### Gmail App Password

To send verification emails, you need a **Gmail App Password** (not your regular password):

1. Enable [2-Step Verification](https://myaccount.google.com/security) on your Google account
2. Go to [App Passwords](https://myaccount.google.com/apppasswords)
3. Generate a new app password for "Mail"
4. Use the generated 16-character password as `EMAIL_PASSWORD`

---

### 5. Database Setup

```bash
# 1. Open PostgreSQL shell (psql) and create the database:
psql -U postgres
CREATE DATABASE neurolens_db;
\q

# 2. The tables are auto-created when the backend starts (via SQLAlchemy)

# 3. (Optional) Create an admin user:
python create_admin.py
```

---

## Running the Application

You need **two terminals** running simultaneously:

### Terminal 1 — Start the Backend

```bash
cd NeuroLens_FYP

# Activate virtual environment
.\venv\Scripts\Activate          # Windows PowerShell
# source venv/bin/activate       # macOS/Linux

# Run the FastAPI server
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

The API will be available at: **http://localhost:8000**  
Swagger docs at: **http://localhost:8000/docs**

### Terminal 2 — Start the Frontend

```bash
cd NeuroLens_FYP/neurolens_front

# Run on Chrome (recommended)
flutter run -d chrome

# Or run on Edge
flutter run -d edge

# Or run on Windows desktop (requires Visual Studio with C++ workload)
flutter run -d windows
```

### (Optional) Terminal 3 — Activity Module

```bash
cd NeuroLens_FYP/Activity_Module

# Install activity module dependencies (if separate venv)
pip install -r requirements.txt

# Run the activity classifier service
uvicorn main:app --reload --port 8001
```

---

## Features

### 🔐 Authentication & Security
- User registration with **email verification** (6-digit OTP via Gmail)
- Secure login with **JWT tokens** and **Argon2** password hashing
- **Forgot password** flow with email-based reset codes
- All personally identifiable information (PII) encrypted at rest using **Fernet encryption**
- Login attempt **lockout** after failed attempts
- Guest mode for quick access

### 📸 Real-Time Emotion Detection
- Live webcam feed with **frame-by-frame emotion analysis**
- Powered by **DeepFace** deep learning model
- Detects 7 emotions: Happy, Sad, Angry, Fear, Surprise, Disgust, Neutral
- Multi-face detection with automatic session pause
- Emotion intensity scoring

### 🖥️ Screen Activity Classification
- Classifies active screen content using a multi-model AI pipeline:
  - **CLIP** — Fast zero-shot image classification
  - **Florence-2** — Detailed image captioning
  - **Qwen** — Contextual activity labeling
- Detects activity categories (e.g., Coding, Social Media, Gaming, etc.)

### 📊 Dashboard & Reports
- Real-time emotion timeline with **animated charts**
- Session-based emotion summaries and trends
- Detailed reports with emotion distribution breakdowns
- Historical data analysis

### 🔔 Smart Notifications & Recommendations
- Detects prolonged negative emotional states
- Delivers **personalized content recommendations** (wellness tips, break suggestions)
- Session-aware notification system

### 👤 User Profile Management
- Editable profile with email change verification
- Password change with current password validation
- Admin dashboard for user management

---

## API Endpoints

| Method | Endpoint                     | Description                    |
| ------ | ---------------------------- | ------------------------------ |
| POST   | `/api/auth/signup/initiate`  | Start signup with email OTP    |
| POST   | `/api/auth/signup/verify`    | Verify email and complete signup |
| POST   | `/api/auth/login`            | Login and get JWT token        |
| POST   | `/api/auth/guest`            | Guest login                    |
| POST   | `/api/analyze/frame`         | Analyze a webcam frame         |
| POST   | `/api/recording/start`       | Start analysis session         |
| POST   | `/api/recording/stop`        | Stop analysis session          |
| GET    | `/api/recommendations`       | Get personalized recommendations |
| GET    | `/api/report`                | Get user emotion reports       |
| GET    | `/api/profile`               | Get user profile               |
| PUT    | `/api/profile/update`        | Update profile                 |
| POST   | `/api/auth/forgot-password`  | Request password reset         |
| POST   | `/api/auth/reset-password`   | Reset password with OTP        |

> Full interactive API documentation available at **http://localhost:8000/docs** when the backend is running.

---

## Contributors

| Name | Role |
|------|------|
| **Nouman Arif** | Backend Engineer |
| **Umaim Tahir** | App Developer |
| **Abdul Samee** | App Developer |

---

## License

This project was developed as a **Final Year Project (FYP)** for academic purposes.

---

<p align="center">
  Made with ❤️ by the NeuroLens Team
</p>
