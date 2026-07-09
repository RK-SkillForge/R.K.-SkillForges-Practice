
# 🤝 Contributing to R.K. SkillForge

> Welcome! Whether this is your **first open source contribution** or your hundredth — you're in the right place. This guide explains everything step by step, so you never feel lost.

---

## 📋 Table of Contents

- [Who Can Contribute?](#-ways-to-contribute)
- [Ways to Contribute](#-ways-to-contribute)
- [Before You Start](#-before-you-start)
- [Setting Up Your Environment](#-setting-up-your-environment)
- [Branching Strategy](#-branching-strategy)
- [Making Changes](#-making-changes)
- [Commit Message Rules](#-commit-message-rules)
- [Opening a Pull Request (PR)](#-opening-a-pull-request-pr)
- [Code Review Process](#-code-review-process)
- [Reporting Bugs](#-reporting-bugs)
- [Suggesting Features](#-suggesting-features)
- [Coding Standards](#-coding-standards)
- [Adding Logs to Your Code](#-adding-logs-to-your-code)
- [Writing Tests](#-writing-tests)
- [Getting Help](#-getting-help)

---

## 👋 Who Can Contribute?

**Everyone is welcome.** You do not need to be an expert.

- 🆕 **Beginners** — start with issues tagged [`good first issue`](https://github.com/your-username/R.K. SkillForge/labels/good%20first%20issue)
- 🐛 **Bug hunters** — find and fix bugs tagged [`bug`](https://github.com/your-username/R.K. SkillForge/labels/bug)
- ✨ **Feature builders** — work on issues tagged [`enhancement`](https://github.com/your-username/R.K. SkillForge/labels/enhancement)
- 📝 **Writers** — improve docs, guides, and comments
- 🎨 **Designers** — improve UI/UX components
- 🧪 **Testers** — write unit/integration tests

---

## 🌟 Ways to Contribute

| Type            | What to do                                                    |
| --------------- | ------------------------------------------------------------- |
| Fix a bug       | Comment on the issue → fork → fix → PR                     |
| Add a feature   | Open a feature request first → get approval → build         |
| Improve docs    | Edit any`.md` file, fix typos, add examples                 |
| Write tests     | Add tests to`backend/tests/` or `frontend/src/__tests__/` |
| Review PRs      | Leave helpful, respectful feedback on open PRs                |
| Report a bug    | Use the Bug Report issue template                             |
| Suggest an idea | Use the Feature Request issue template                        |

---

## ✅ Before You Start

1. **Read the README** — understand what R.K. SkillForge is and how to run it locally.
2. **Search existing issues** — check if someone is already working on the same thing.
3. **Comment on the issue** — write "I'd like to work on this" so others know it's taken.
4. **Wait for a maintainer to assign it to you** — this avoids duplicate work.

> ⚠️ **Do not open a PR for something no one asked for** without first opening an issue and discussing it. It may not get merged.

---

## 🛠 Setting Up Your Environment

### Step 1 — Fork the repository

1. Go to `https://github.com/your-username/R.K. SkillForge`
2. Click **Fork** (top-right)
3. Choose your GitHub account

### Step 2 — Clone your fork

```bash
git clone https://github.com/YOUR-GITHUB-USERNAME/R.K. SkillForge.git
cd R.K. SkillForge
```

### Step 3 — Add upstream remote

This lets you pull in new changes from the original repo later:

```bash
git remote add upstream https://github.com/original-owner/R.K. SkillForge.git

# Confirm it worked
git remote -v
```

You should see:

```
origin    https://github.com/YOUR-USERNAME/R.K. SkillForge.git
upstream  https://github.com/original-owner/R.K. SkillForge.git
```

### Step 4 — Install dependencies

```bash
# Backend
cd backend
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt

# Frontend
cd ../frontend
npm install
```

### Step 5 — Set up environment variables

```bash
cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env
```

Fill in the `.env` values — see [README.md](README.md) for all variables explained.

### Step 6 — Run the project locally

```bash
# Start everything with Docker (easiest)
docker-compose up --build

# OR manually — run these in separate terminals:
# Terminal 1: Backend
cd backend && uvicorn app.main:app --reload --port 8000

# Terminal 2: Celery worker
cd backend && celery -A celery_worker worker --loglevel=info

# Terminal 3: Frontend
cd frontend && npm run dev
```

Verify it's working:

- Frontend: http://localhost:5173
- API docs: http://localhost:8000/docs

---

## 🌿 Branching Strategy

**Never commit directly to `main`.** Always create a new branch for your work.

### Branch naming format

```
type/short-description
```

| Type          | When to use                  | Example                            |
| ------------- | ---------------------------- | ---------------------------------- |
| `feature/`  | Adding new functionality     | `feature/add-video-likes`        |
| `fix/`      | Fixing a bug                 | `fix/auth-token-not-refreshing`  |
| `docs/`     | Documentation only           | `docs/update-ssh-guide`          |
| `test/`     | Adding/fixing tests          | `test/auth-service-unit-tests`   |
| `refactor/` | Code cleanup, no new feature | `refactor/video-service-cleanup` |
| `chore/`    | Dependencies, config, build  | `chore/upgrade-fastapi-version`  |

### How to create your branch

```bash
# First, make sure your main is up to date
git checkout main
git pull upstream main

# Create and switch to your new branch
git checkout -b feature/your-feature-name

# Example:
git checkout -b fix/comment-delete-404-error
```

---

## ✏️ Making Changes

### Backend changes (FastAPI)

Follow this flow depending on what you're changing:

| What                       | Where                             |
| -------------------------- | --------------------------------- |
| New API endpoint           | `backend/app/api/v1/endpoints/` |
| Business logic             | `backend/app/services/`         |
| Database query             | `backend/app/repositories/`     |
| MongoDB document shape     | `backend/app/models/`           |
| API request/response shape | `backend/app/schemas/`          |
| Background job             | `backend/app/tasks/`            |
| AWS utility                | `backend/app/utils/`            |

### Frontend changes (React)

| What               | Where                        |
| ------------------ | ---------------------------- |
| New page/screen    | `frontend/src/pages/`      |
| Reusable component | `frontend/src/components/` |
| API calls          | `frontend/src/services/`   |
| Redux state        | `frontend/src/features/`   |
| Custom hook        | `frontend/src/hooks/`      |
| Helper function    | `frontend/src/utils/`      |

---

## 💬 Commit Message Rules

We follow the **Conventional Commits** standard. Every commit message must follow this format:

```
type: short description in lowercase
```

### Commit types

| Type         | Use when                    | Example                                  |
| ------------ | --------------------------- | ---------------------------------------- |
| `feat`     | Adding new feature          | `feat: add video like endpoint`        |
| `fix`      | Fixing a bug                | `fix: token refresh returning 401`     |
| `docs`     | Documentation changes       | `docs: fix typo in README`             |
| `test`     | Adding or fixing tests      | `test: add auth service unit tests`    |
| `refactor` | Restructuring code          | `refactor: move feed logic to service` |
| `style`    | Formatting, no logic change | `style: run black formatter on auth`   |
| `chore`    | Build, deps, config         | `chore: upgrade motor to 3.5.0`        |
| `perf`     | Performance improvement     | `perf: add index to video creator_id`  |

### Rules

- ✅ Use lowercase
- ✅ Keep it under 72 characters
- ✅ Write in present tense ("add" not "added")
- ❌ Do not end with a period
- ❌ Do not use vague messages like `fix stuff` or `update code`

### Good vs bad examples

```bash
# ✅ Good
git commit -m "feat: add like and dislike to video endpoint"
git commit -m "fix: playlist not saving when video_ids is empty"
git commit -m "docs: add environment variable table to README"

# ❌ Bad
git commit -m "fixed it"
git commit -m "WIP"
git commit -m "changes"
git commit -m "Update"
```

---

## 🔃 Opening a Pull Request (PR)

### Step 1 — Push your branch

```bash
git push origin feature/your-feature-name
```

### Step 2 — Open the PR on GitHub

1. Go to your fork on GitHub
2. You'll see a banner: **"Compare & pull request"** — click it
3. Set the base branch to `main` (original repo)
4. Fill in the PR template (see below)

### PR Title format

Same as commit messages:

```
feat: add video like/dislike endpoint
fix: playlist not saving with empty video list
docs: add logging guide to docs folder
```

### PR Description — what to include

```markdown
## What does this PR do?
Adds like and dislike functionality to the video endpoint.
Users can toggle their reaction. Counts update in real time.

## Related issue
Closes #42

## Type of change
- [x] Bug fix
- [ ] New feature
- [ ] Documentation
- [ ] Refactor

## How to test
1. Run the backend locally
2. POST /api/v1/videos/{id}/like with a valid JWT
3. Confirm the like count increments
4. POST again to confirm it toggles off

## Screenshots (if UI change)
[attach screenshot here]

## Checklist
- [x] Tests pass locally
- [x] Code formatted with black / ESLint
- [x] No console.log or print() left in code
- [x] .env.example updated if new env vars added
```

### Step 3 — Respond to review comments

- Be open to feedback — it's not personal, it's about the code
- Make requested changes, then push to the same branch (PR updates automatically)
- Reply to each comment after addressing it

---

## 🔍 Code Review Process

1. A maintainer reviews your PR within **48–72 hours**
2. You may receive change requests — that's completely normal
3. After all checks pass and approval is given, it gets merged
4. Your name will be added to `CONTRIBUTORS.md` 🎉

### What reviewers check

- Does it solve the stated problem?
- Is the code readable and well-organized?
- Are there tests for the new logic?
- Does it follow the project's file structure?
- Are there any security issues?
- Are logs added where needed?

---

## 🐛 Reporting Bugs

Found something broken? Great — that's a valuable contribution too.

### Before reporting

1. Check if the bug is already reported in [Issues](https://github.com/your-username/R.K. SkillForge/issues)
2. Check you're running the latest version
3. Try to reproduce it consistently

### How to report

1. Go to **Issues → New Issue → Bug Report**
2. Fill in the template completely:

```markdown
**Bug description**
Video like endpoint returns 500 when video_id doesn't exist.

**Steps to reproduce**
1. POST /api/v1/videos/invalid_id/like
2. Send valid JWT token
3. See 500 Internal Server Error

**Expected behavior**
Should return 404 with message "Video not found"

**Actual behavior**
Returns 500 with no error message

**Environment**
- OS: Ubuntu 22.04
- Python: 3.11.2
- Branch: main

**Logs**
Paste relevant logs here
```

---

## 💡 Suggesting Features

Have an idea? We'd love to hear it.

1. Go to **Issues → New Issue → Feature Request**
2. Describe the feature clearly:
   - What problem does it solve?
   - Who benefits from it?
   - How should it work?
3. Wait for maintainer feedback before building it

---

## 🧹 Coding Standards

### Python (Backend)

```bash
# Format code
black .

# Sort imports
isort .

# Lint
flake8 .
```

Rules:

- Follow PEP 8
- Use type hints on all functions
- Keep functions under 40 lines
- Add docstrings to service and repository functions

```python
# ✅ Good
async def get_video_by_id(video_id: str) -> VideoResponse:
    """Fetch a single video by its MongoDB ID.
  
    Args:
        video_id: The MongoDB ObjectId as a string.
  
    Returns:
        VideoResponse schema with video data.
  
    Raises:
        HTTPException 404 if video not found.
    """
    video = await video_repo.get_by_id(video_id)
    if not video:
        raise HTTPException(status_code=404, detail="Video not found")
    return VideoResponse(**video.dict())
```

### JavaScript/React (Frontend)

```bash
# Lint
npm run lint

# Format (if Prettier is set up)
npm run format
```

Rules:

- Use functional components with hooks — no class components
- One component per file
- Use named exports (not default) for components
- No `console.log` left in production code (use proper logging)

---

## 📝 Adding Logs to Your Code

**Do not use `print()` in Python or `console.log` in JavaScript for production code.**
Use the structured logger instead — it adds timestamps, log levels, and context.

### Backend — Python with Loguru

```python
from loguru import logger

# At the top of your service file
class VideoService:
  
    async def upload_video(self, data: VideoCreate, user_id: str):
        logger.info(f"Video upload started | user_id={user_id} | title={data.title}")
    
        try:
            video = await video_repo.create(data)
            logger.info(f"Video created in DB | video_id={video.id}")
        
            await sqs.enqueue_video_processing(str(video.id))
            logger.info(f"Video queued for processing | video_id={video.id}")
        
            return video
        
        except Exception as e:
            logger.error(f"Video upload failed | user_id={user_id} | error={str(e)}")
            raise
```

### Log levels — when to use which

| Level                 | Use for                            | Example                                        |
| --------------------- | ---------------------------------- | ---------------------------------------------- |
| `logger.debug()`    | Dev details, not shown in prod     | `logger.debug(f"DB query: {query}")`         |
| `logger.info()`     | Normal flow milestones             | `logger.info(f"User registered: {user_id}")` |
| `logger.warning()`  | Something odd but not broken       | `logger.warning(f"Slow query: {elapsed}ms")` |
| `logger.error()`    | Something failed but app continues | `logger.error(f"Email send failed: {e}")`    |
| `logger.critical()` | App about to crash                 | `logger.critical("DB connection lost")`      |

### What to always log

- ✅ Service function start: "User X started Y"
- ✅ Key milestones: "Video created", "Email sent", "S3 upload complete"
- ✅ All errors with context: what failed + why + who triggered it
- ✅ Authentication events: login, logout, failed attempt
- ❌ Never log passwords, tokens, or credit card numbers

### Frontend — React

```javascript
// Create a simple logger util at frontend/src/utils/logger.js
const isDev = import.meta.env.DEV;

export const logger = {
  info:  (...args) => isDev && console.info('[INFO]', ...args),
  warn:  (...args) => console.warn('[WARN]', ...args),
  error: (...args) => console.error('[ERROR]', ...args),
};

// Usage in components or services
import { logger } from '../utils/logger';

logger.info('Video fetch started', { videoId });
logger.error('Video fetch failed', error.message);
```

---

## 🧪 Writing Tests

All PRs that add new logic must include tests.

### Backend tests

```bash
cd backend
pytest tests/ -v                           # run all tests
pytest tests/unit/ -v                      # unit tests only
pytest tests/ --cov=app --cov-report=term  # with coverage
```

Minimum coverage requirement: **80%**

```python
# Example: backend/tests/unit/test_video_service.py
import pytest
from unittest.mock import AsyncMock, patch
from app.services.video_service import VideoService

@pytest.mark.asyncio
async def test_upload_video_success():
    mock_repo = AsyncMock()
    mock_repo.create.return_value = {"id": "abc123", "title": "Test Video"}
  
    service = VideoService(repo=mock_repo)
    result = await service.upload_video(
        data=VideoCreate(title="Test Video"),
        user_id="user_001"
    )
  
    assert result["id"] == "abc123"
    mock_repo.create.assert_called_once()
```

### Frontend tests

```bash
cd frontend
npm run test
npm run test -- --coverage
```

---

## 🆘 Getting Help

Stuck? These are the best ways to get help:

- 💬 **Comment on the issue** you're working on — maintainers check regularly
- 🐛 **Open a new issue** with the label `question` if it's a general question
- 📚 **Check the docs** in the `docs/` folder — especially `SSH_SETUP.md` and `LOGGING_GUIDE.md`
- 🔍 **Search closed issues** — your question may already be answered

Please be patient. Maintainers are volunteers and will respond as soon as they can.

---

## 🙏 Thank You

Every contribution — big or small — makes R.K. SkillForge better for everyone.
We appreciate your time and effort. 💙
