# naxcv

**naxcv** is a simple, modern, beginner-friendly AI resume builder inspired by the FastResume AI prompt. It lets users upload an old resume, paste or upload a job description, calculate an ATS score, receive AI-style improvement suggestions, edit resume sections, choose a template, and export a PDF.

The app is intentionally clean and easy to understand, and it is organized so you can expand it later.

## Features

- Register, login, logout
- Upload PDF or DOCX resumes
- Extract resume text and basic profile information
- Paste or upload a job description
- Extract important job keywords
- Compare resume with job description
- Show ATS score, keyword match, missing skills, and improvement tips
- AI improvement suggestions with Google Gemini when an API key is configured
- Local fallback suggestions when no API key is available
- Resume builder with live preview
- Auto-save while editing
- Three templates: Professional, Modern, Minimal
- Download final resume as PDF
- Profile storage in SQLite
- Dark and light mode

## Tech Stack

### Frontend

- HTML5
- CSS3
- JavaScript
- Bootstrap 5

### Backend

- Python
- FastAPI
- SQLite
- SQLAlchemy
- Google Gemini Free API support
- pdfplumber
- python-docx
- reportlab

## Project Structure

```text
naxcv/
├── frontend/
│   ├── index.html
│   ├── login.html
│   ├── register.html
│   ├── dashboard.html
│   ├── upload.html
│   ├── builder.html
│   ├── profile.html
│   ├── css/
│   │   ├── style.css
│   │   ├── dashboard.css
│   │   └── auth.css
│   ├── js/
│   │   ├── app.js
│   │   ├── auth.js
│   │   ├── upload.js
│   │   └── builder.js
│   └── assets/
│       └── logo.svg
├── backend/
│   ├── main.py
│   ├── database.py
│   ├── models.py
│   ├── auth.py
│   ├── parser.py
│   ├── ai.py
│   ├── ats.py
│   ├── resume.py
│   ├── requirements.txt
│   ├── .env.example
│   ├── uploads/
│   ├── generated/
│   └── database.db       # created automatically when the server starts
├── README.md
└── .gitignore
```

## Quick Start

### 1. Open the backend folder

```bash
cd naxcv/backend
```

### 2. Create and activate a virtual environment

```bash
python -m venv .venv
```

On macOS/Linux:

```bash
source .venv/bin/activate
```

On Windows:

```bash
.venv\Scripts\activate
```

### 3. Install packages

```bash
pip install -r requirements.txt
```

### 4. Configure environment variables

```bash
cp .env.example .env
```

Edit `.env`:

```env
SECRET_KEY=change-this-secret-key
GEMINI_API_KEY=your-free-gemini-api-key
GEMINI_MODEL=gemini-1.5-flash
ALLOWED_ORIGINS=*
```

`GEMINI_API_KEY` is optional. If you leave it empty, the app still works with local keyword matching and rule-based suggestions.

### 5. Start the backend

```bash
uvicorn main:app --reload
```

Backend URL:

```text
http://127.0.0.1:8000
```

API docs:

```text
http://127.0.0.1:8000/docs
```

### 6. Open the frontend

You have two easy options:

1. Open the served frontend from FastAPI:

```text
http://127.0.0.1:8000
```

2. Or open `frontend/index.html` directly in your browser.

If your backend URL is different, set it in the browser console:

```js
localStorage.setItem('naxcv_api_base', 'https://your-backend-url.com');
```

Then refresh the page.

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| POST | `/register` | Create a new user account |
| POST | `/login` | Login and receive an auth token |
| POST | `/upload-resume` | Upload PDF/DOCX/TXT resume and extract information |
| POST | `/upload-job-description` | Upload or paste a job description and extract keywords |
| POST | `/analyze` | Compare resume and job description, calculate ATS score, and generate suggestions |
| POST | `/generate-resume` | Generate a resume PDF from builder/profile data |
| GET | `/profile` | Get saved profile |
| PUT | `/profile` | Update saved profile |
| GET | `/history` | Get previous resume uploads/analyses |
| DELETE | `/resume/{id}` | Delete a saved resume record |

## Example User Flow

1. Register or login.
2. Upload an old resume.
3. Paste or upload a job description.
4. The backend extracts resume information and job keywords.
5. Click **Analyze Resume**.
6. View ATS score, keyword match, missing skills, and improvement tips.
7. Open the builder.
8. Edit resume sections with live preview.
9. Choose Professional, Modern, or Minimal template.
10. Download the final PDF.

## Free Deployment Idea

### Backend on Render

- Create a free Render web service.
- Root directory: `backend`
- Build command:

```bash
pip install -r requirements.txt
```

- Start command:

```bash
uvicorn main:app --host 0.0.0.0 --port $PORT
```

- Add environment variables from `.env.example`.

### Frontend on Vercel

- Import the GitHub repository into Vercel.
- Set the frontend directory as the project root if needed.
- Update API base in `frontend/js/app.js`, or set it with localStorage in the browser.

## Notes for Beginners

- The backend code uses simple helper functions and comments so it is easy to understand.
- Passwords are hashed with Python standard library tools.
- Tokens are lightweight signed tokens built with standard library modules.
- SQLite is enough for learning and small demos.
- The Gemini integration is optional and isolated in `backend/ai.py`.
- The ATS scoring is transparent and easy to customize in `backend/ats.py`.

## Important Security Notes

This is a beginner-friendly project, not a production security template. Before using it for real users, improve token management, add rate limiting, use HTTPS, validate uploads more strictly, and store generated files securely.
