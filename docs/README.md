# Mentor Scoring System - AI-Powered Session Analysis Platform

> A comprehensive full-stack application that analyzes mentor presentation sessions using AI, providing detailed feedback on communication metrics, engagement patterns, and areas for improvement.

## Table of Contents

- [Project Overview](#project-overview)
- [Architecture](#architecture)
- [Setup Instructions](#setup-instructions)
- [Running Locally](#running-locally)
- [API Endpoints](#api-endpoints)
- [Example Inputs/Outputs](#example-inputsoutputs)
- [Dependencies](#dependencies)
- [Contributors](#contributors)

---

## Project Overview

### What is the Mentor Scoring System?

The Mentor Scoring System is an intelligent platform designed to evaluate and improve mentor effectiveness through AI-powered video analysis. It captures mentor presentations, analyzes them across multiple dimensions (clarity, engagement, pacing, gestures, eye contact), and provides actionable feedback for improvement.

### Key Features

**Video Upload & Processing**
- Support for direct file uploads (MP4, AVI, MOV, MKV, FLV, WMV, WEBM, M4V)
- YouTube URL integration with automatic downloading
- Cloud storage via Cloudinary (no ephemeral filesystem issues)

**AI Analysis Engine**
- External services for video analysis and speaker diarization
- Automatic metric extraction (6 core metrics: Clarity, Engagement, Pacing, Eye Contact, Gestures, Overall)
- Gemini AI integration for intelligent feedback generation
- Weak moment identification and improvement suggestions

**Comprehensive Dashboards**
- Real-time session breakdown visualization
- Timeline-based audio/video/transcript analysis
- Score dips and peaks identification
- Public leaderboard with mentor rankings

**AI Voice Clone**
- Text-to-speech generation using AI voice models
- ngrok integration for external AI services
- Audio download and playback capabilities

**Secure Architecture**
- Backend-signed Cloudinary uploads (API secret never exposed)
- CORS-enabled for cross-origin requests
- MongoDB Atlas for persistent data storage

---

## Architecture

### System Architecture Diagram

![System Architecture - Mentor Scoring System](./Architecture_diagram.jpeg)

#### 1. Login Page
![Login Page](./login_page.png)

```
┌─────────────────────────────────────────────────────────────────┐
│                    Frontend (Vercel)                             │
│  Next.js 16 + React 19 + TypeScript + Tailwind CSS             │
│  ├─ Mentor Dashboard                                            │
│  ├─ Session Upload & Analysis                                  │
│  ├─ Session Breakdown Visualization                            │
│  ├─ AI Voice Clone Interface                                   │
│  └─ Public Leaderboard                                         │
└─────────────────────────────────────────────────────────────────┘
                            │
                  ┌─────────┼─────────┐
                  │         │         │
            ┌─────▼─────────▼─────────▼─────────────────┐
            │   External Services (Cloud)               │
            ├─ Cloudinary Video Storage                 │
            ├─ Analysis API (Video Understanding)       │
            ├─ Diarization API (Speaker Detection)      │
            ├─ AI Voice Clone Service (ngrok tunnel)    │
            └─────────────────────────────────────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
   ┌────▼──────────────────▼───────────────────▼────────────────┐
   │        Backend (Render) - Flask + Python 3.11              │
   ├─────────────────────────────────────────────────────────────┤
   │ API Endpoints:                                             │
   │ ├─ /api/auth/* (Login, Register)                          │
   │ ├─ /api/mentor/* (Dashboard, Skills, Sessions)            │
   │ ├─ /api/mentor/<id>/sessions/upload (FormData upload)     │
   │ ├─ /api/mentor/<id>/sessions/analyze (URL-based analysis) │
   │ ├─ /api/cloudinary/signature (Signed upload params)       │
   │ ├─ /api/cloudinary/delete (Video deletion)                │
   │ └─ /api/public/* (Public rankings, profiles)              │
   │                                                            │
   │ Components:                                                │
   │ ├─ cloudinary_handler.py (Cloud storage management)       │
   │ ├─ models.py (Database models + Gemini integration)       │
   │ ├─ app.py (Flask routes and business logic)               │
   │ └─ db_setup.py (MongoDB initialization)                   │
   └─────────────────────────────────────────────────────────────┘
        │
        └─────────────────────────────┬──────────────────────────┐
                              ┌───────▼────────┐         ┌──────▼───────┐
                              │ MongoDB Atlas  │         │ Gemini API   │
                              │ (Persistent)   │         │ (AI Summary) │
                              └────────────────┘         └──────────────┘
```

#### 2. Mentor Dashboard
![Mentor Dashboard](./mentor_dashboard.png)

#### 3. Mentor Dashboard - Sessions View
![Mentor Dashboard 2](./mentor_dashboard_2.png)

#### 4. Mentor Session Breakdown - Main View
![Session Breakdown](./mentor_session_breakdown.png)

#### 5. Mentor Session Breakdown - Detailed Analysis
![Session Breakdown 2](./mentor_session_breakdown_2.png)

#### 6. Mentor Session Breakdown - Advanced Metrics
![Session Breakdown 3](./mentor_session_breakdown_3.png)

#### 7. Mentor Session Breakdown - Timeline View
![Session Breakdown 4](./mentor_session_breakdown_4.png)

#### 8. Mentor Session Breakdown - Final Summary
![Session Breakdown 5](./mentor_session_breakdown_5.png)

#### 9. Mentor Profile
![Mentor Profile](./mentor_profile.png)

#### 10. Public Dashboard Leaderboard
![Public Dashboard](./public_dashboard.png)

#### 11. AI Voice Clone - Student Dashboard
![AI Voice Clone - Student Dashboard](./mentor_ai_voice_clone_student_dashboard.png)

### Data Flow

1. **Upload Phase**
   - User selects video (file or YouTube URL)
   - Frontend uploads to Cloudinary first
   - Gets secure URL from Cloudinary
   - Sends URL to backend for analysis

2. **Analysis Phase**
   - Backend receives video URL
   - Calls Analysis Service → metrics extraction
   - Calls Diarization Service → speaker detection
   - Uses Gemini to generate insights
   - Stores complete session in MongoDB

3. **Retrieval Phase**
   - Frontend fetches session breakdown
   - Backend normalizes data and fills missing fields
   - Renders timeline, metrics, and weak moments

### Key Technologies

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Frontend** | Next.js 16, React 19, TypeScript | Modern UI framework |
| **Backend** | Flask 3.0, Python 3.11 | API server |
| **Database** | MongoDB Atlas | Session storage |
| **Storage** | Cloudinary | Video hosting |
| **AI** | Google Gemini 2.5 Flash | Feedback generation |
| **Deployment** | Vercel (Frontend), Render (Backend) | Cloud hosting |

---

## Tech Stack & Evaluation Pipeline

Our ML evaluation system scores a session across five core dimensions, each powered by specialized models and signal-processing techniques.

---

### 1. Engagement (20%)
- Q&A pair extraction
- Interaction relevance modeling
- Response–engagement scoring

---

### 2. Communication (20%)
- Librosa-based stuttering detection
- Pause duration analysis
- Fluency & speech-rate modeling

---

### 3. Technical Depth (30%)
- Semantic search on transcript
- Topic–scope mapping within video
- Depth & correctness scoring

---

### 4. Clarity (20%)
#### Audio/Video Quality Analysis
- Audio quality heuristics
- Energy & pitch variation modeling
- OpenCV + OpenFace for:
  - Gaze tracking
  - Head pose estimation

#### Visual Understanding
- Lightweight CNN for gesture frequency
- Tesseract OCR for on-screen text
- Detectron2 for layout detection

---

### 5. Interaction (10%)
- Two-way engagement
- Responsiveness patterns
- User–teacher interaction depth

---

## Teacher Clone Module

We build a GPT-style personalized educator using:
- Extracted teaching patterns
- Communication style embedding
- Domain expertise fingerprints

Output: A generated Teacher Persona that can produce scripts, answer questions, and interact with students like the original mentor.

---

## Setup Instructions

### Prerequisites

- **Node.js** 18+ (for frontend)
- **Python** 3.11+ (for backend)
- **MongoDB Atlas** account (for database)
- **Cloudinary** account (for video storage)
- **Google Gemini API** key (for AI analysis)
- **Git** for version control

### Example Credentials (for testing)

⚠️ **Note:** User registration is not available. You must login using the predefined credentials below. There is no self-signup endpoint.

Use these example accounts for local testing or demo purposes only. Do NOT use them in production.

- Mentor: `mentor@example.com` / `mentor123`
- Student: `student@example.com` / `student123`
- University: `university@@example.com` / `university123`

### Backend Setup

#### 1. Clone Repository

```bash
cd backend-new
```

#### 2. Create Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

#### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

#### 4. Configure Environment Variables

Create `.env` file in `backend-new/`:

```env
# Database
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/dbname

# Cloudinary
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret

# AI/APIs
GEMINI_API_KEY=your_gemini_api_key

# Server
FLASK_ENV=development
FLASK_PORT=5000
```

#### 5. Initialize Database

```bash
python db_setup.py
```

#### 6. Run Backend Server

```bash
python app.py
```

Backend will run on `http://localhost:5000`

### Frontend Setup

#### 1. Navigate to Frontend Directory

```bash
cd frontend
```

#### 2. Install Dependencies

```bash
npm install
```

#### 3. Configure Environment Variables

Create `.env` file in `frontend/`:

```env
NEXT_PUBLIC_CLOUDINARY_CLOUD_NAME=your_cloud_name
NEXT_PUBLIC_API_URL=http://localhost:5000
```

#### 4. Run Frontend Development Server

```bash
npm run dev
```

Frontend will run on `http://localhost:3000`

---

## Running Locally

### Complete Local Setup

```bash
# Terminal 1: Backend
cd backend-new
source venv/bin/activate
python app.py

# Terminal 2: Frontend (new terminal)
cd frontend
npm run dev
```

### Testing the Application

1. **Access Application**: Open `http://localhost:3000`

2. **Create Account**: Register as mentor or student

3. **Upload Video**:
   - Go to Dashboard → Create New Session
   - Choose: File Upload or YouTube URL
   - Add session name and context
   - Click "Create Session"

4. **View Analysis**:
   - Dashboard shows session card
   - Click "View Breakdown" to see detailed analysis
   - Explore metrics, timeline, and weak moments

5. **Test AI Voice Clone**:
   - Navigate to any mentor's AI Clone page
   - Enter text
   - Click "Generate Audio"
   - Listen and download generated audio

### Troubleshooting

| Issue | Solution |
|-------|----------|
| CORS errors | Ensure backend CORS is configured properly |
| Cloudinary upload fails | Verify API key and secret in `.env` |
| Gemini API errors | Check API key validity and quota |
| MongoDB connection fails | Verify connection string and IP whitelist |
| Render free-tier timeouts / cold starts | The backend is hosted on Render's free tier which may spin down idle instances. The first 2–3 requests after idle can time out while the instance spins up. Retry the request after a few seconds; consider using a warmup/ping service or upgrading the instance for production. |
| Video not playable (timed-out or loading issues) | If the backend was asleep or request timed out during processing, video playback links may fail temporarily. Wait a few seconds and reload the session page. Re-uploading the video or re-processing the session usually resolves the issue. |
| Large ML model / tunneling in use | The ML model Docker image is ~15GB and cannot be hosted on the free instance directly. For demos we use tunneling (external ML host or remote tunnel). This means analysis may be delayed or routed through a tunnel — see deployment notes for details. |

---

## API Endpoints

### Authentication Endpoints

#### Login
```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "mentor@example.com",
  "password": "password123"
}
```

**Response** (200 OK):
```json
{
  "user": {
    "id": "user123",
    "email": "mentor@example.com",
    "name": "John Mentor",
    "role": "mentor"
  },
  "token": "jwt_token_here"
}
```

### Session Endpoints

#### Upload Session (FormData)
```http
POST /api/mentor/{mentor_id}/sessions/upload
Content-Type: multipart/form-data

form data:
- file: <video_file>
- sessionName: "Teaching Best Practices"
- context: "Q3 Mentoring Session"
- userId: "user123"
```

**Response** (201 Created):
```json
{
  "message": "Upload received. Analysis and diarization called.",
  "session": {
    "id": "session_abc123",
    "sessionId": "session_abc123",
    "sessionName": "Teaching Best Practices",
    "videoUrl": "https://res.cloudinary.com/...",
    "duration": 1800,
    "metrics": [
      {
        "name": "Clarity",
        "score": 85,
        "confidenceInterval": [80, 90],
        "whatHelped": ["Clear structure", "Good examples"],
        "whatHurt": ["Some jargon", "Too fast middle"]
      }
    ],
    "timeline": {
      "audio": [...],
      "video": [...],
      "transcript": [...],
      "scoreDips": [...],
      "scorePeaks": [...]
    },
    "weakMoments": [
      {
        "timestamp": "00:12:30",
        "message": "Speaking speed increased - slow down"
      }
    ]
  }
}
```

#### Analyze Video from URL
```http
POST /api/mentor/{mentor_id}/sessions/analyze
Content-Type: application/json

{
  "videoUrl": "https://res.cloudinary.com/...",
  "context": "Session analysis with context",
  "sessionName": "Q3 Mentoring",
  "userId": "user123",
  "uploadMode": "file"
}
```

**Response** (201 Created): Same as upload endpoint

#### Get Session Breakdown
```http
GET /api/mentor/{mentor_id}/sessions/{session_id}/breakdown
Authorization: Bearer token
```

**Response** (200 OK):
```json
{
  "sessionId": "session_abc123",
  "sessionName": "Teaching Best Practices",
  "duration": 1800,
  "metrics": [...],
  "timeline": {...},
  "weakMoments": [...]
}
```

#### Get Mentor Sessions
```http
GET /api/mentor/{mentor_id}/sessions
Authorization: Bearer token
```

**Response** (200 OK):
```json
{
  "sessions": [
    {
      "id": "session_abc123",
      "sessionName": "Teaching Best Practices",
      "date": "2025-12-10T10:30:00Z",
      "score": 85,
      "weakMoments": [...],
      "studentCount": 25
    }
  ]
}
```

### Cloudinary Endpoints

#### Get Upload Signature
```http
POST /api/cloudinary/signature
Content-Type: application/json

{
  "mentorId": "mentor123",
  "sessionId": "session_abc123"
}
```

**Response** (200 OK):
```json
{
  "signature": "sha1_hash_signature",
  "timestamp": 1702310400,
  "api_key": "your_api_key",
  "public_id": "mentor_videos/mentor123/session_abc123"
}
```

#### Delete Video
```http
POST /api/cloudinary/delete
Content-Type: application/json

{
  "publicId": "mentor_videos/mentor123/session_abc123"
}
```

**Response** (200 OK):
```json
{
  "message": "Video deleted successfully"
}
```

### Public Endpoints

#### Get Public Rankings
```http
GET /api/public/mentors/rankings?subject=Communication&experience=expert
```

**Response** (200 OK):
```json
{
  "filters": {
    "subjects": ["Communication", "Leadership"],
    "languages": ["English", "Spanish"],
    "experienceLevels": ["beginner", "intermediate", "expert"]
  },
  "rankings": [
    {
      "rank": 1,
      "name": "Jane Expert",
      "verified": true,
      "overallScore": 92,
      "strengthTag": "Excellent Clarity",
      "avgScoreTrend": [80, 85, 88, 92]
    }
  ]
}
```

#### Get Public Mentor Profile
```http
GET /api/public/mentors/{mentor_id}
```

**Response** (200 OK):
```json
{
  "id": "mentor123",
  "name": "Jane Expert",
  "email": "jane@example.com",
  "verified": true,
  "subject": "Communication",
  "experience": "expert",
  "averageScore": 92,
  "sessionCount": 45,
  "topMetrics": ["Clarity", "Engagement"],
  "recentSessions": [...]
}
```

---

## 📊 Example Inputs/Outputs

### Example 1: Video Upload with Analysis

**Input:**
```
User uploads a 30-minute mentor presentation video about "Leadership in Remote Teams"
```

**Processing:**
1. Video uploaded to Cloudinary (~5-10 seconds)
2. Analysis service analyzes video content (~30-60 seconds)
3. Diarization service identifies speakers (~20-40 seconds)
4. Gemini generates insights and feedback (~10-20 seconds)
5. Session saved to MongoDB (~2-3 seconds)

**Output:**
```json
{
  "sessionId": "session_7d3f9a2b",
  "sessionName": "Leadership in Remote Teams",
  "duration": 1800,
  "metrics": [
    {
      "name": "Clarity",
      "score": 82,
      "confidenceInterval": [77, 87],
      "whatHelped": [
        "Clear structure with three main points",
        "Used real-world examples effectively",
        "Defined technical terms before use"
      ],
      "whatHurt": [
        "Spoke too fast in the middle section",
        "Some slides had too much text",
        "Didn't pause for questions enough"
      ]
    },
    {
      "name": "Engagement",
      "score": 78,
      "confidenceInterval": [73, 83],
      "whatHelped": [
        "Asked audience questions at breakpoints",
        "Varied tone and pacing",
        "Made eye contact with camera"
      ],
      "whatHurt": [
        "Limited audience participation",
        "Some long pauses lost attention",
        "Could use more interactive elements"
      ]
    }
  ],
  "timeline": {
    "audio": [
      {
        "startTime": 0,
        "endTime": 300,
        "pace": 140,
        "pauses": 2,
        "type": "normal",
        "message": "Good opening with clear introduction"
      },
      {
        "startTime": 600,
        "endTime": 900,
        "pace": 180,
        "pauses": 0,
        "type": "fast",
        "message": "Speaking speed increased, hard to follow"
      }
    ],
    "scoreDips": [
      {
        "timestamp": 720,
        "score": 72,
        "message": "Speaking too fast, clarity decreased",
        "type": "audio"
      }
    ],
    "scorePeaks": [
      {
        "timestamp": 150,
        "score": 88,
        "message": "Excellent opening with strong clarity",
        "type": "overall"
      }
    ]
  },
  "weakMoments": [
    {
      "timestamp": "00:12:00",
      "message": "Slow down delivery in technical section"
    },
    {
      "timestamp": "00:18:30",
      "message": "Engage audience more - ask direct questions"
    }
  ]
}
```

### Example 2: AI Voice Clone Text-to-Speech

**Input:**
```json
{
  "text": "Today we'll discuss effective mentoring techniques for remote teams...",
  "name": "striver",
  "language": "en",
  "temperature": 0.8,
  "repetition_penalty": 1.2,
  "cfg_weight": 0.5,
  "exaggeration": 0.5,
  "top_p": 1,
  "min_p": 0.05
}
```

**Processing:**
- External AI Voice service processes text with mentor's voice parameters
- Generates natural speech audio file

**Output:**
```
Audio Blob: [WAV format, ~5-10 seconds, ~100KB]
```

**Download:**
```
File: mentor-audio-1702310400123.wav
Size: ~100KB
Duration: 8 seconds
Format: WAV, 44.1kHz, Mono
```

### Example 3: Leaderboard Data

**Input:**
```
GET /api/public/mentors/rankings?subject=Communication&experience=expert
```

**Output:**
```json
{
  "rankings": [
    {
      "rank": 1,
      "name": "Alexandra Chen",
      "verified": true,
      "overallScore": 94,
      "strengthTag": "Expert Clarity",
      "avgScoreTrend": [85, 87, 90, 92, 94]
    },
    {
      "rank": 2,
      "name": "Marcus Johnson",
      "verified": true,
      "overallScore": 91,
      "strengthTag": "Excellent Engagement",
      "avgScoreTrend": [80, 83, 87, 89, 91]
    },
    {
      "rank": 3,
      "name": "Priya Sharma",
      "verified": false,
      "overallScore": 88,
      "strengthTag": "Strong Pacing",
      "avgScoreTrend": [75, 79, 83, 85, 88]
    }
  ]
}
```

---

## 📦 Dependencies

### Frontend Dependencies

```json
{
  "core": {
    "next": "16.0.7",
    "react": "19.2.0",
    "react-dom": "19.2.0",
    "typescript": "^5"
  },
  "ui-components": {
    "@radix-ui/react-dropdown-menu": "^2.1.16",
    "@radix-ui/react-label": "^2.1.8",
    "@radix-ui/react-select": "^2.2.6",
    "@radix-ui/react-slot": "^1.2.4",
    "lucide-react": "^0.556.0"
  },
  "styling": {
    "tailwindcss": "^4",
    "tailwind-merge": "^3.4.0",
    "class-variance-authority": "^0.7.1",
    "clsx": "^2.1.1"
  },
  "features": {
    "embla-carousel-react": "^8.6.0",
    "recharts": "^3.5.1",
    "next-themes": "^0.4.6"
  }
}
```

### Backend Dependencies

```python
# Web Framework
flask==3.0.0
flask-cors==4.0.0

# Video Processing
yt-dlp>=2024.12.13
moviepy==1.0.3

# Database
pymongo==4.6.0

# Cloud Storage
cloudinary>=1.36.0

# HTTP & Utilities
werkzeug==3.0.1
requests>=2.31.0
python-dotenv==1.0.0

# Optional: AI Integration
# google-generativeai (for Gemini API)
```

### External Services

| Service | Purpose | Version |
|---------|---------|---------|
| MongoDB Atlas | Database | Latest |
| Cloudinary | Video Storage | API v1.1 |
| Google Gemini | AI Analysis | 2.5-flash |
| Analysis API | Video Understanding | Custom |
| Diarization API | Speaker Detection | Custom |

---

## 👥 Contributors

### Project Team

- **Himanshu** - Lead Developer
  - Frontend architecture (Next.js, React, TypeScript)
  - Backend API design and Flask implementation
  - Database schema and MongoDB integration
  - Deployment to Vercel and Render

- **Contributors**
  - AI integration with Google Gemini
  - Cloudinary integration for video storage
  - API signature generation for security
  - Frontend component library implementation

### Acknowledgments

- **IIT Bombay** - Hackathon platform
- **HCL Technologies** - Technical sponsorship
- **Google Cloud** - Gemini API access
- **Cloudinary** - Video hosting infrastructure

### Contact & Support

For issues, questions, or contributions:
- **GitHub**: [mentor-scoring-system](https://github.com/Himanshu1198/)
- **Email**: himanshu@example.com
- **Issues**: [GitHub Issues](https://github.com/Himanshu1198/Final_version_techfest_IIT_bombay_HCL/issues)

---

## 📄 License

This project is part of the IIT Bombay TechFest 2025 and HCL Hackathon.

---

## 🔄 Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | Dec 2025 | Initial release with full video analysis |
| 0.9.0 | Dec 2025 | Cloudinary integration and signed uploads |
| 0.8.0 | Dec 2025 | Backend-signed upload implementation |
| 0.7.0 | Dec 2025 | CORS improvements and URL formatting fixes |
| 0.6.0 | Dec 2025 | AI Voice Clone text-to-speech integration |
| 0.5.0 | Dec 2025 | Complete video analysis pipeline |

---

## 🎯 Roadmap

### Current Release (v1.0)
✅ Video upload and analysis
✅ AI feedback generation
✅ Mentor dashboards
✅ Public leaderboards
✅ AI voice clone

### Next Release (v1.1)
🔲 Real-time analysis progress tracking
🔲 Batch session processing
🔲 Custom analysis templates
🔲 Advanced export options (PDF reports)

### Future Enhancements (v2.0)
🔲 Live session analysis
🔲 Multi-language support
🔲 Team performance analytics
🔲 Mobile app (React Native)
🔲 Enhanced AI models

---

**Last Updated**: December 12, 2025  
**Status**: Active Development  
**Maintained By**: Himanshu & Team
