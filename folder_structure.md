
# 📁 R.K. SkillForge — Folder Structure Guide

> A complete walkthrough of every folder and file in the project, organized by layer.

---

## 🗂️ Root Overview

```
R.K. SkillForge/
├── frontend/               ← React.js UI (what users see)
├── backend/                ← FastAPI REST API (business logic)
├── infrastructure/         ← AWS + Terraform + Docker (deployment)
├── .github/                ← CI/CD pipelines (GitHub Actions)
├── docker-compose.yml      ← Run everything locally in one command
├── .gitignore
└── README.md
```

---

## ⚛️ Frontend — `frontend/`

Built with **React.js + Vite**. Everything the user interacts with lives here.

```
frontend/
├── public/
│   ├── index.html              ← App entry HTML
│   └── favicon.ico
│
├── src/
│   │
│   ├── assets/                 ← Images, icons, custom fonts
│   │
│   ├── components/             ← Reusable UI pieces (used across multiple pages)
│   │   ├── common/             ← Generic: Button, Input, Modal, Loader, Avatar, Badge
│   │   ├── layout/             ← App shell: Header, Sidebar, Footer, PageWrapper
│   │   ├── video/              ← VideoPlayer, VideoCard, VideoUploader, VideoActions
│   │   ├── course/             ← CourseCard, CourseList, CoursePricing
│   │   ├── comment/            ← CommentList, CommentInput
│   │   ├── feed/               ← FeedItem (personalized content card)
│   │   ├── chatbot/            ← ChatWindow, ChatMessage (AI assistant UI)
│   │   └── subscription/       ← SubscribeButton component
│   │
│   ├── pages/                  ← One folder per screen/route
│   │   ├── Home/               ← Landing page + recommended feed
│   │   ├── Auth/
│   │   │   ├── Login/          ← Email + Google OAuth login
│   │   │   └── Register/       ← Sign up page
│   │   ├── Profile/
│   │   │   ├── ViewProfile/    ← Public profile view
│   │   │   └── EditProfile/    ← Edit your own profile
│   │   ├── Video/
│   │   │   ├── WatchVideo/     ← Video player page (like/dislike/share/comment)
│   │   │   └── UploadVideo/    ← Creator video upload (async processing)
│   │   ├── Course/
│   │   │   ├── BrowseCourses/  ← Discover free & paid courses
│   │   │   ├── CourseDetail/   ← Course info + enroll
│   │   │   └── CreateCourse/   ← Creator: build a course
│   │   ├── Creator/
│   │   │   └── Dashboard/      ← Creator dashboard (upload + browse simultaneously)
│   │   ├── Feed/               ← Personalized content feed based on interests
│   │   ├── Chatbot/            ← AI chatbot page (science & spirituality Q&A)
│   │   ├── Playlist/           ← Playlist management
│   │   └── NotFound/           ← 404 page
│   │
│   ├── features/               ← Redux Toolkit state slices (one per domain)
│   │   ├── auth/               ← authSlice.js, authActions.js
│   │   ├── user/               ← userSlice.js
│   │   ├── video/              ← videoSlice.js
│   │   ├── course/             ← courseSlice.js
│   │   ├── feed/               ← feedSlice.js
│   │   ├── chatbot/            ← chatbotSlice.js
│   │   └── subscription/       ← subscriptionSlice.js
│   │
│   ├── hooks/                  ← Custom React hooks
│   │   ├── useAuth.js          ← Auth state + JWT token refresh
│   │   ├── useVideo.js         ← Video CRUD & streaming
│   │   ├── useFeed.js          ← Infinite scroll feed
│   │   └── useDebounce.js      ← Debounce search/filter inputs
│   │
│   ├── services/               ← All API calls (Axios) — one file per domain
│   │   ├── api.js              ← Axios base instance + request/response interceptors
│   │   ├── authService.js      ← login, register, refresh token
│   │   ├── userService.js      ← get profile, update profile
│   │   ├── videoService.js     ← upload, fetch, like, share
│   │   ├── courseService.js    ← browse, enroll, create, price
│   │   ├── feedService.js      ← fetch personalized feed
│   │   └── chatbotService.js   ← send message to AI chatbot
│   │
│   ├── store/
│   │   └── index.js            ← Redux store + combined root reducer
│   │
│   ├── routes/
│   │   ├── index.jsx           ← All route definitions
│   │   ├── PrivateRoute.jsx    ← Redirect unauthenticated users → /login
│   │   └── CreatorRoute.jsx    ← Restrict pages to creator role only
│   │
│   ├── utils/                  ← Pure helper functions
│   │   ├── formatDate.js       ← "2024-01-15" → "Jan 15, 2024"
│   │   ├── formatDuration.js   ← 754 seconds → "12:34"
│   │   └── validators.js       ← Email, password, URL validation
│   │
│   ├── constants/
│   │   ├── routes.js           ← Route path constants (avoids magic strings)
│   │   └── config.js           ← App-wide config values
│   │
│   ├── context/
│   │   └── ThemeContext.jsx    ← Light / dark theme state (React Context)
│   │
│   ├── App.jsx                 ← Root component — router + global providers
│   └── main.jsx                ← Vite entry point, mounts React to DOM
│
├── .env                        ← VITE_API_BASE_URL, VITE_GOOGLE_CLIENT_ID
├── .env.example                ← Template — copy this to create your own .env
├── package.json
└── vite.config.js              ← Path aliases, dev proxy → :8000, env setup
```

---

## 🐍 Backend — `backend/`

Built with **FastAPI + Motor (async MongoDB) + Beanie ODM**.

```
backend/
├── app/
│   │
│   ├── main.py                 ← FastAPI app instance, CORS, middleware, lifespan events
│   ├── config.py               ← Pydantic Settings — reads all .env variables
│   ├── database.py             ← Motor async MongoDB client + Beanie ODM initialization
│   │
│   ├── api/
│   │   └── v1/
│   │       ├── router.py       ← Aggregates all endpoint routers into one
│   │       └── endpoints/      ← One file per API feature area
│   │           ├── auth.py         ← POST /register /login /google-auth /refresh
│   │           ├── users.py        ← GET PATCH /users/{id} — profile management
│   │           ├── videos.py       ← Upload, stream, like/dislike, share
│   │           ├── courses.py      ← Create, price, enroll, browse (free & paid)
│   │           ├── playlists.py    ← Create playlist, add/remove videos
│   │           ├── comments.py     ← Post, reply, delete comments
│   │           ├── subscriptions.py ← Subscribe/unsubscribe to creator
│   │           ├── feed.py         ← GET /feed — personalized content
│   │           └── chatbot.py      ← POST /chatbot/message — AI Q&A
│   │
│   ├── core/                   ← Security, OAuth, shared dependencies
│   │   ├── security.py         ← JWT token create/validate, bcrypt password hashing
│   │   ├── oauth.py            ← Google OAuth2 "Continue with Google" flow
│   │   ├── dependencies.py     ← FastAPI Depends(): get_current_user(), require_creator_role()
│   │   └── exceptions.py       ← Custom HTTP exception classes + global handlers
│   │
│   ├── models/                 ← MongoDB document schemas (Beanie ODM)
│   │   │                          These define the actual collections in MongoDB
│   │   ├── user.py             ← User: auth info, profile, role (viewer/creator), interests[]
│   │   ├── video.py            ← Video: title, s3_url, likes, comments, playlist_ids
│   │   ├── course.py           ← Course: type (free/paid), price, external_link, sections[]
│   │   ├── playlist.py         ← Playlist: name, owner_id, video_ids[], visibility
│   │   ├── comment.py          ← Comment: video_id, user_id, text, replies[]
│   │   └── subscription.py     ← Subscription: follower_id → creator_id
│   │
│   ├── schemas/                ← Pydantic request/response validation models
│   │   │                          These define what the API accepts and returns
│   │   ├── auth.py             ← LoginRequest, RegisterRequest, TokenResponse
│   │   ├── user.py             ← UserCreate, UserUpdate, UserPublic
│   │   ├── video.py            ← VideoCreate, VideoUpdate, VideoResponse
│   │   ├── course.py           ← CourseCreate, CourseUpdate, CourseResponse
│   │   └── comment.py          ← CommentCreate, CommentResponse
│   │
│   ├── services/               ← Business logic layer (keeps endpoints thin)
│   │   ├── auth_service.py     ← Register, login, token refresh, Google OAuth
│   │   ├── user_service.py     ← Profile CRUD, role management
│   │   ├── video_service.py    ← Upload trigger, S3 pre-signed URL, like/dislike
│   │   ├── course_service.py   ← Course CRUD, enrollment, pricing
│   │   ├── feed_service.py     ← Assemble personalized feed from interest graph
│   │   ├── recommendation_service.py ← Interest-based content scoring + ranking
│   │   └── chatbot_service.py  ← LLM API call — science & spirituality Q&A
│   │
│   ├── repositories/           ← Raw MongoDB queries, isolated from business logic
│   │   ├── base_repo.py        ← Generic async CRUD: get, create, update, delete, list
│   │   ├── user_repo.py        ← User-specific queries (find by email, etc.)
│   │   ├── video_repo.py       ← Video queries (trending, by creator, etc.)
│   │   └── course_repo.py      ← Course queries (by category, price range, etc.)
│   │
│   ├── tasks/                  ← Celery async background jobs
│   │   ├── video_processing.py ← Transcode video + upload to S3 after creator submits
│   │   ├── thumbnail_gen.py    ← Auto-generate thumbnails from video frame (Pillow)
│   │   └── notifications.py    ← Push/email: new subscriber, comment, course update
│   │
│   └── utils/                  ← AWS helper utilities
│       ├── aws_s3.py           ← boto3: multipart upload, pre-signed GET/PUT URLs
│       ├── aws_cloudfront.py   ← CloudFront signed URL for private video delivery
│       ├── aws_sqs.py          ← Enqueue/poll video processing jobs via SQS
│       └── helpers.py          ← Slug generator, pagination helpers, etc.
│
├── tests/
│   ├── unit/                   ← Unit tests for services and utilities
│   └── integration/            ← Integration tests against real MongoDB
│
├── celery_worker.py            ← Celery worker entry — picks up async video jobs from SQS
├── requirements.txt            ← All Python dependencies with pinned versions
├── Dockerfile                  ← Multi-stage Python build for production
└── .env                        ← MONGODB_URI, JWT_SECRET, AWS_*, GOOGLE_CLIENT_*
```

### 📐 How Models, Schemas, Services & Repositories work together

```
HTTP Request
    │
    ▼
[ endpoints/ ]          ← receives request, calls service
    │
    ▼
[ services/ ]           ← business logic, orchestrates repos
    │
    ▼
[ repositories/ ]       ← raw MongoDB queries using models
    │
    ▼
[ models/ ]             ← Beanie ODM → actual MongoDB collection
    │
    ▼
[ schemas/ ]            ← Pydantic validates input + shapes output
    │
    ▼
HTTP Response
```

> **Rule of thumb:** Endpoints stay thin (just routing). Services hold the logic. Repositories hold the queries. Models define the DB shape. Schemas define the API shape.

---

## ☁️ Infrastructure — `infrastructure/`

**AWS resources managed with Terraform. Docker for containerization.**

```
infrastructure/
│
├── terraform/
│   ├── modules/                ← Reusable Terraform module per AWS service
│   │   ├── s3/                 ← 100TB video & asset storage
│   │   │                          (lifecycle policies, encryption, versioning)
│   │   ├── cloudfront/         ← CDN for global low-latency video delivery
│   │   │                          (handles 200TB/day bandwidth)
│   │   ├── ec2/                ← EC2/ECS app servers
│   │   │                          (auto-scales to handle 200 peak RPS)
│   │   ├── elasticache/        ← Redis — cache feeds, sessions, recommendations
│   │   ├── sqs/                ← Job queue — decouples creators from processing
│   │   └── vpc/                ← VPC, private subnets, security groups, NAT gateway
│   │
│   ├── environments/
│   │   ├── dev/                ← Dev: smaller instances, relaxed policies
│   │   └── prod/               ← Prod: multi-AZ, strict IAM, full monitoring
│   │
│   └── main.tf                 ← Root — wires all modules together
│
├── docker/
│   ├── frontend.Dockerfile     ← Node build stage → nginx static serve
│   ├── backend.Dockerfile      ← Python multi-stage — strips dev dependencies
│   └── nginx.conf              ← Reverse proxy: /api → backend:8000, / → frontend:80
│
└── scripts/
    ├── deploy.sh               ← Build Docker images, push to ECR, apply Terraform
    └── setup_env.sh            ← Bootstrap dev env — install tools, create .env files
```

---

## 🔄 CI/CD — `.github/`

**GitHub Actions pipelines that run automatically on every push/PR.**

```
.github/
└── workflows/
    ├── frontend-ci.yml     ← Runs on every PR: lint → type-check → test → build
    ├── backend-ci.yml      ← Runs on every PR: pytest → coverage check (min 80%)
    └── deploy.yml          ← Runs on merge to main: build → push → deploy to AWS
```

---

## 🔑 Key Architecture Decisions

| Decision                      | What we chose              | Why                                             |
| ----------------------------- | -------------------------- | ----------------------------------------------- |
| Async MongoDB                 | Motor + Beanie ODM         | Non-blocking DB calls for high concurrency      |
| Separated schemas from models | `schemas/` + `models/` | API shape ≠ DB shape — allows flexibility     |
| Repository layer              | `repositories/`          | Keeps MongoDB queries isolated and testable     |
| Background tasks              | Celery + SQS               | Video processing is slow — don't block the API |
| CDN for videos                | CloudFront                 | 200TB/day bandwidth, global low latency         |
| Caching                       | Redis (ElastiCache)        | Fast feed/recommendation results at 100k DAU    |

---

## 📝 Quick File Lookup

| I want to…                       | Go to…                               |
| --------------------------------- | ------------------------------------- |
| Add a new API endpoint            | `backend/app/api/v1/endpoints/`     |
| Change MongoDB schema             | `backend/app/models/`               |
| Change API request/response shape | `backend/app/schemas/`              |
| Add business logic                | `backend/app/services/`             |
| Add a DB query                    | `backend/app/repositories/`         |
| Add a background job              | `backend/app/tasks/`                |
| Add a new page                    | `frontend/src/pages/`               |
| Add a reusable component          | `frontend/src/components/`          |
| Add an API call from frontend     | `frontend/src/services/`            |
| Change Redux state                | `frontend/src/features/`            |
| Add an AWS resource               | `infrastructure/terraform/modules/` |
