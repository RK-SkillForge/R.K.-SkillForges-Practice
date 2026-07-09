# R.K.-SkillForges-Practice


<div align="center">

---

## 📖 Table of Contents

- [About the Project](#-about-the-project)
- [Tech Stack](#-tech-stack)
- [System Requirements](#-system-requirements)
- [Getting Started](#-getting-started)
- [How to Fork](#-how-to-fork-this-project)
- [Contributing](#-contributing)
- [Project Structure](#-project-structure)
- [License](#-license)

---

## 🌟 About the Project

R.K. SkillForges is a full-stack online learning platform where creators can upload videos and courses (free or paid), and learners can discover content based on their interests.

**Core features:**

- 📹 Video upload and streaming (async processing via Celery + AWS S3)
- 📚 Free and paid course creation with playlist support
- 👤 Creator and viewer roles with subscription system
- 💬 Like, dislike, share, and comment on content
- 🔍 Personalized feed and recommendations based on interests
- 🤖 AI chatbot (science & spirituality Q&A)
- 🔐 Email/password + Google OAuth authentication

**Scale target:** 1M users · 100k DAU · 200 peak RPS · 100TB storage

---

## 🛠 Tech Stack

| Layer          | Technology                                  |
| -------------- | ------------------------------------------- |
| Frontend       | React.js 18, Vite, Redux Toolkit, Axios     |
| Backend        | FastAPI, Python 3.11, Uvicorn               |
| Database       | MongoDB 7 (Motor async driver + Beanie ODM) |
| Cache          | Redis (AWS ElastiCache)                     |
| Task Queue     | Celery + AWS SQS                            |
| Storage        | AWS S3 (100TB), CloudFront CDN              |
| Infrastructure | AWS EC2/ECS, Terraform                      |
| CI/CD          | GitHub Actions                              |
| Auth           | JWT + bcrypt, Google OAuth2                 |

---

## 💻 System Requirements

Make sure you have these installed before starting:

| Tool              | Version | Download            |
| ----------------- | ------- | ------------------- |
| Python            | 3.11+   | https://python.org  |
| Node.js           | 18+     | https://nodejs.org  |
| MongoDB           | 7.0+    | https://mongodb.com |
| Redis             | 7.0+    | https://redis.io    |
| Git               | Any     | https://git-scm.com |
| Docker (optional) | 24+     | https://docker.com  |

---

## 🚀 Getting Started

### Option A — Run with Docker (easiest, one command)

```bash
# 1. Clone the repository
git clone https://github.com/your-username/R.K. SkillForges.git
cd R.K. SkillForges

# 2. Copy environment files
cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env

# 3. Start everything (frontend + backend + MongoDB + Redis)
docker-compose up --build
```

Open **http://localhost:5173** in your browser. Done! ✅

---

### Option B — Run manually (step by step)

#### Step 1 — Clone the repository

```bash
git clone https://github.com/your-username/R.K. SkillForges.git
cd R.K. SkillForges
```

---

#### Step 2 — Set up the Backend

```bash
# Go to backend folder
cd backend

# Create a virtual environment
python -m venv venv

# Activate virtual environment
# macOS / Linux:
source venv/bin/activate
# Windows:
venv\Scripts\activate

# Install all dependencies
pip install -r requirements.txt

# Copy environment file and fill in your values
cp .env.example .env
```

Open `backend/.env` and fill in:

```env
MONGODB_URI=mongodb://localhost:27017/R.K. SkillForges
JWT_SECRET=your_super_secret_key_here
JWT_ALGORITHM=HS256
JWT_EXPIRE_MINUTES=60

GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret

AWS_ACCESS_KEY_ID=your_aws_key
AWS_SECRET_ACCESS_KEY=your_aws_secret
AWS_REGION=ap-south-1
S3_BUCKET_NAME=R.K. SkillForges-videos

OPENAI_API_KEY=your_openai_key

REDIS_URL=redis://localhost:6379
```

Start the FastAPI server:

```bash
uvicorn app.main:app --reload --port 8000
```

API is running at **http://localhost:8000**
Swagger docs at **http://localhost:8000/docs** ✅

---

#### Step 3 — Start the Celery Worker (video processing)

Open a **new terminal**, activate venv again:

```bash
cd backend
source venv/bin/activate   # or venv\Scripts\activate on Windows
celery -A celery_worker worker --loglevel=info
```

---

#### Step 4 — Set up the Frontend

Open **another new terminal**:

```bash
# Go to frontend folder
cd frontend

# Install dependencies
npm install

# Copy environment file
cp .env.example .env
```

Open `frontend/.env` and fill in:

```env
VITE_API_BASE_URL=http://localhost:8000
VITE_GOOGLE_CLIENT_ID=your_google_client_id
```

Start the React dev server:

```bash
npm run dev
```

Frontend is running at **http://localhost:5173** ✅

---

#### Step 5 — Verify everything is running

| Service     | URL                        | Status check                      |
| ----------- | -------------------------- | --------------------------------- |
| Frontend    | http://localhost:5173      | Should show R.K. SkillForges home |
| Backend API | http://localhost:8000/docs | Should show Swagger UI            |
| MongoDB     | localhost:27017            | Check with MongoDB Compass        |
| Redis       | localhost:6379             | Check with`redis-cli ping`      |

---

## 🍴 How to Fork This Project

Forking creates your own copy of R.K. SkillForges so you can make changes without affecting the original.

### Step 1 — Fork on GitHub

1. Go to **https://github.com/your-username/R.K. SkillForges**
2. Click the **Fork** button (top-right of the page)
3. Select your GitHub account as the destination
4. GitHub creates `https://github.com/YOUR-USERNAME/R.K. SkillForges`

### Step 2 — Clone your fork locally

```bash
git clone https://github.com/YOUR-USERNAME/R.K. SkillForges.git
cd R.K. SkillForges
```

### Step 3 — Add the original repo as "upstream"

This lets you pull in future updates from the main project:

```bash
git remote add upstream https://github.com/original-owner/R.K. SkillForges.git

# Verify your remotes
git remote -v
# origin    https://github.com/YOUR-USERNAME/R.K. SkillForges.git (fetch)
# origin    https://github.com/YOUR-USERNAME/R.K. SkillForges.git (push)
# upstream  https://github.com/original-owner/R.K. SkillForges.git (fetch)
# upstream  https://github.com/original-owner/R.K. SkillForges.git (push)
```

### Step 4 — Sync your fork with latest changes (anytime)

```bash
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

---

## 🤝 Contributing

We welcome all contributions — bug fixes, new features, documentation, tests, and design improvements!

### Before you start

- Read through [folder_structure.md](folder_structure.md) to understand the codebase
- Check [open issues](https://github.com/your-username/R.K. SkillForges/issues) — pick one tagged `good first issue`
- Comment on the issue to let others know you're working on it

---

### Contribution Workflow

#### Step 1 — Fork and clone (see above)

#### Step 2 — Create a feature branch

**Never commit directly to `main`.** Always create a new branch:

```bash
# Checkout main and pull latest
git checkout main
git pull upstream main

# Create your feature branch
git checkout -b feature/your-feature-name

# Examples:
# git checkout -b feature/add-video-likes
# git checkout -b fix/auth-token-refresh
# git checkout -b docs/update-api-readme
```

#### Step 3 — Make your changes

Follow the structure below depending on what you're changing:

| What you're working on | Where to look                                                    |
| ---------------------- | ---------------------------------------------------------------- |
| New API endpoint       | `backend/app/api/v1/endpoints/` + `services/` + `schemas/` |
| New MongoDB model      | `backend/app/models/` + `repositories/`                      |
| New React page         | `frontend/src/pages/`                                          |
| New reusable component | `frontend/src/components/`                                     |
| Frontend API call      | `frontend/src/services/`                                       |
| Redux state            | `frontend/src/features/`                                       |

#### Step 4 — Write tests

```bash
# Backend tests
cd backend
pytest tests/ -v

# Run with coverage
pytest tests/ --cov=app --cov-report=term-missing
```

All PRs must pass tests with **minimum 80% coverage**.

#### Step 5 — Check code quality

```bash
# Backend
cd backend
black .           # auto-format code
isort .           # sort imports
flake8 .          # check for linting errors

# Frontend
cd frontend
npm run lint      # ESLint check
npm run build     # verify it builds without errors
```

#### Step 6 — Commit with a clear message

Follow this format:

```bash
git add .
git commit -m "type: short description"

# Types:
# feat     → new feature
# fix      → bug fix
# docs     → documentation only
# refactor → code change, no new feature or bug fix
# test     → adding or updating tests
# chore    → build/config changes

# Examples:
git commit -m "feat: add video like/dislike endpoint"
git commit -m "fix: token refresh not updating cookie"
git commit -m "docs: add environment variable guide"
```

#### Step 7 — Push and open a Pull Request

```bash
git push origin feature/your-feature-name
```

Then go to GitHub → your fork → click **"Compare & pull request"**.

Fill in the PR template:

- What does this PR do?
- Which issue does it close? (write `Closes #123`)
- Screenshots / test results (if UI change)

---

### PR Review Process

1. A maintainer reviews your PR within **48 hours**
2. You may be asked to make changes — that's normal!
3. Once approved, it gets merged into `main`
4. Your contribution appears in the `CONTRIBUTORS.md`

---

### Good First Issues

Looking for where to start? Try these:

- [ ] Add pagination to the video feed API
- [ ] Add loading skeleton components to the Feed page
- [ ] Write unit tests for `auth_service.py`
- [ ] Add a "Copy link" share button to VideoActions
- [ ] Improve error messages in the registration form
- [ ] Add dark mode toggle to the Header

---

## 📁 Project Structure

See [folder_structure.md](folder_structure.md) for a complete breakdown of every folder and file.

Quick reference:

```
R.K. SkillForges/
├── frontend/         ← React.js (pages, components, services, hooks)
├── backend/          ← FastAPI (endpoints, models, schemas, services, repos)
├── infrastructure/   ← Terraform + Docker + scripts
└── .github/          ← GitHub Actions CI/CD
```

---

## 📜 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

You are free to:

- ✅ Use this project for personal and commercial purposes
- ✅ Modify and distribute it
- ✅ Fork it and build your own product on top of it

---

## 🙏 Acknowledgements

Built as part of the **R.K. R.K. SkillForges** system design project.

If this project helped you, please ⭐ **star the repo** — it helps others find it!

---

<div align="center">
  Made with ❤️ by the R.K. SkillForges contributors
</div>
