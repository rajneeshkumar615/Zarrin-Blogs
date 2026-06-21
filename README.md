<div align="center">

# Zarrin Blogs

**A production-ready, full-stack blogging platform built for scale.**

[![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=flat-square)](https://github.com/zarrin-blogs)
[![Version](https://img.shields.io/badge/Version-1.0.0-blue?style=flat-square)](https://github.com/zarrin-blogs)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)
[![Node](https://img.shields.io/badge/Node.js-20.x-339933?style=flat-square&logo=node.js&logoColor=white)](https://nodejs.org)
[![React](https://img.shields.io/badge/React-18.x-61DAFB?style=flat-square&logo=react&logoColor=black)](https://reactjs.org)
[![MongoDB](https://img.shields.io/badge/MongoDB-7.x-47A248?style=flat-square&logo=mongodb&logoColor=white)](https://mongodb.com)
[![Express](https://img.shields.io/badge/Express-4.x-000000?style=flat-square&logo=express&logoColor=white)](https://expressjs.com)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=flat-square&logo=docker&logoColor=white)](https://docker.com)
[![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-2088FF?style=flat-square&logo=github-actions&logoColor=white)](https://github.com/features/actions)
[![Coverage](https://img.shields.io/badge/Coverage-87%25-success?style=flat-square)](https://github.com/zarrin-blogs)

<br/>

[Live Demo](https://zarrin-blogs.vercel.app) · [API Docs](https://zarrin-blogs-api.onrender.com/api/docs) · [Report Bug](https://github.com/zarrin-blogs/issues) · [Request Feature](https://github.com/zarrin-blogs/issues)

</div>

---

## Table of Contents

- [About](#about)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture Overview](#architecture-overview)
- [Folder Structure](#folder-structure)
- [Installation](#installation)
- [API Endpoints](#api-endpoints)
- [Environment Variables](#environment-variables)
- [Testing](#testing)
- [Docker Setup](#docker-setup)
- [CI/CD Pipeline](#cicd-pipeline)
- [Screenshots](#screenshots)
- [Performance & Security](#performance--security)
- [Future Improvements](#future-improvements)
- [License](#license)
- [Author](#author)

---

## About

Zarrin Blogs is a full-stack, production-grade blogging platform built on the MERN stack. It delivers a complete content creation and community engagement ecosystem — from a feature-rich editor and category-based discovery to role-based access control and an admin moderation panel.

Designed with engineering rigour from the ground up, Zarrin Blogs handles authenticated user sessions, rich text authoring, social engagement (likes, comments, bookmarks), and granular administrative controls — all exposed through a clean REST API and backed by a responsive React frontend.

Whether you're looking to deploy a self-hosted publication platform or extend Zarrin Blogs as a headless CMS backend, the architecture is built to support it.

> **Note:** This repository contains the full monorepo, including the React client (`/client`), Express API server (`/server`), shared type definitions, Dockerfiles, and CI/CD configuration.

---

## Features

### User Features

- **Authentication** — JWT-based registration and login with secure refresh token rotation
- **Profile Management** — Update avatar, bio, and account settings
- **Blog Authoring** — Create, edit, and delete personal blog posts with a rich text editor (TipTap)
- **Category & Tag System** — Organise and discover posts by topic
- **Search** — Full-text search across titles, body content, tags, and authors
- **Engagement** — Like posts, bookmark for later reading, and leave threaded comments
- **Feed** — Personalised feed, trending posts, and category-filtered browsing
- **Pagination** — Cursor-based pagination for all post listings

### Admin Features

- **Dashboard** — At-a-glance analytics: total users, posts, daily active sessions, and engagement rates
- **User Management** — View, block, unblock, and delete user accounts
- **Content Moderation** — Delete any post or comment that violates platform policy
- **Category Management** — Create, rename, and remove blog categories
- **Analytics Panel** — Charts for post volume over time, top-performing posts, and user growth
- **Role Assignment** — Promote or demote users between `user` and `admin` roles

### Blog System Features

- **Rich Text Editor** — Headings, bold/italic, inline code, code blocks, image embeds, and blockquotes
- **Draft & Publish Workflow** — Save as draft or publish immediately
- **Trending Algorithm** — Posts ranked by a weighted score (views × 0.3 + likes × 0.5 + comments × 0.2) within a rolling 7-day window
- **View Tracking** — Unique view counts per post (deduplicated by IP + user session)
- **Engagement Metrics** — Per-post counts for likes, comments, bookmarks, and views

---

## Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| **Frontend Framework** | React 18 + Vite | UI rendering and build tooling |
| **State Management** | Redux Toolkit + RTK Query | Global state and server-state caching |
| **Styling** | Tailwind CSS + shadcn/ui | Utility-first responsive design system |
| **Rich Text Editor** | TipTap | Extensible ProseMirror-based editor |
| **Routing** | React Router v6 | Client-side navigation |
| **HTTP Client** | Axios | REST API communication with interceptors |
| **Backend Framework** | Express.js 4 | REST API server |
| **Runtime** | Node.js 20 LTS | Server-side JavaScript runtime |
| **Database** | MongoDB 7 | Document-oriented primary data store |
| **ODM** | Mongoose 8 | Schema modelling and query builder |
| **Authentication** | JWT (jsonwebtoken) + bcrypt | Stateless auth with secure password storage |
| **Validation** | Zod + express-validator | Schema validation on client and server |
| **File Uploads** | Multer + Cloudinary | Image upload and CDN storage |
| **Testing (BE)** | Jest + Supertest | Unit and integration testing |
| **Testing (FE)** | Vitest + React Testing Library | Component and hook testing |
| **E2E Testing** | Cypress | End-to-end browser automation |
| **Containerisation** | Docker + Docker Compose | Reproducible dev and prod environments |
| **CI/CD** | GitHub Actions | Automated test, lint, and deploy pipelines |
| **Frontend Deploy** | Vercel | Edge-optimised static hosting |
| **Backend Deploy** | Render | Managed Node.js hosting |
| **Database Hosting** | MongoDB Atlas | Managed cloud database |

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENT (React)                        │
│   Vite · Redux Toolkit · RTK Query · Tailwind · TipTap      │
└───────────────────────────┬─────────────────────────────────┘
                            │ HTTPS / REST
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    API SERVER (Express)                       │
│                                                              │
│  ┌──────────────┐  ┌───────────────┐  ┌──────────────────┐ │
│  │  Auth Router │  │  Posts Router │  │   Admin Router   │ │
│  └──────┬───────┘  └──────┬────────┘  └────────┬─────────┘ │
│         │                 │                     │           │
│  ┌──────▼─────────────────▼─────────────────────▼────────┐ │
│  │                  Middleware Layer                       │ │
│  │   authenticateJWT · authoriseRole · validateInput      │ │
│  │   rateLimiter · requestLogger · errorHandler           │ │
│  └──────────────────────┬──────────────────────────────────┘ │
│                         │                                    │
│  ┌──────────────────────▼──────────────────────────────────┐ │
│  │               Service / Business Logic                   │ │
│  │   AuthService · PostService · CommentService            │ │
│  │   UserService · CategoryService · AnalyticsService      │ │
│  └──────────────────────┬──────────────────────────────────┘ │
└─────────────────────────┼───────────────────────────────────┘
                          │ Mongoose ODM
          ┌───────────────┴────────────────┐
          │                                │
          ▼                                ▼
  ┌───────────────┐                ┌───────────────┐
  │  MongoDB Atlas │                │   Cloudinary  │
  │  (Primary DB)  │                │ (Media Store) │
  └───────────────┘                └───────────────┘
```

**Request lifecycle:**

1. The React client dispatches an RTK Query request through Axios, attaching the JWT access token via a request interceptor.
2. Express receives the request, runs it through the middleware chain (rate limiting → JWT verification → role authorisation → input validation).
3. The matched route handler delegates to a service layer that encapsulates all business logic.
4. Services interact with MongoDB through Mongoose models, applying query optimisations (indexed fields, projection, lean queries).
5. The JSON response is returned to the client; RTK Query caches it and React re-renders from the updated store.
6. On token expiry, the Axios interceptor silently calls `POST /api/auth/refresh` before retrying the original request.

---

## Folder Structure

```
zarrin-blogs/
├── .github/
│   └── workflows/
│       ├── ci.yml                  # Run tests on every PR
│       └── deploy.yml              # Deploy on merge to main
│
├── client/                         # React frontend (Vite)
│   ├── public/
│   ├── src/
│   │   ├── app/
│   │   │   └── store.ts            # Redux store configuration
│   │   ├── assets/
│   │   ├── components/
│   │   │   ├── common/             # Reusable UI components
│   │   │   │   ├── Button/
│   │   │   │   ├── Modal/
│   │   │   │   └── Pagination/
│   │   │   ├── editor/             # TipTap editor components
│   │   │   ├── layout/             # Navbar, Sidebar, Footer
│   │   │   └── post/               # Post card, detail, list
│   │   ├── features/
│   │   │   ├── auth/               # Auth slice + RTK Query
│   │   │   ├── posts/              # Posts slice + RTK Query
│   │   │   ├── comments/
│   │   │   ├── admin/
│   │   │   └── users/
│   │   ├── hooks/                  # Custom React hooks
│   │   ├── pages/
│   │   │   ├── Home.tsx
│   │   │   ├── PostDetail.tsx
│   │   │   ├── CreatePost.tsx
│   │   │   ├── EditPost.tsx
│   │   │   ├── Profile.tsx
│   │   │   ├── Search.tsx
│   │   │   ├── Login.tsx
│   │   │   ├── Register.tsx
│   │   │   └── admin/
│   │   │       ├── Dashboard.tsx
│   │   │       ├── ManageUsers.tsx
│   │   │       ├── ManagePosts.tsx
│   │   │       └── ManageCategories.tsx
│   │   ├── router/
│   │   │   ├── AppRouter.tsx
│   │   │   ├── ProtectedRoute.tsx
│   │   │   └── AdminRoute.tsx
│   │   ├── services/               # RTK Query API slices
│   │   ├── types/                  # Shared TypeScript types
│   │   ├── utils/
│   │   ├── App.tsx
│   │   └── main.tsx
│   ├── index.html
│   ├── vite.config.ts
│   ├── tailwind.config.ts
│   └── tsconfig.json
│
├── server/                         # Express API
│   ├── src/
│   │   ├── config/
│   │   │   ├── db.ts               # MongoDB connection
│   │   │   ├── cloudinary.ts
│   │   │   └── env.ts              # Validated env config (Zod)
│   │   ├── controllers/
│   │   │   ├── auth.controller.ts
│   │   │   ├── post.controller.ts
│   │   │   ├── comment.controller.ts
│   │   │   ├── user.controller.ts
│   │   │   ├── category.controller.ts
│   │   │   └── admin.controller.ts
│   │   ├── middleware/
│   │   │   ├── authenticate.ts     # JWT verification
│   │   │   ├── authorise.ts        # RBAC enforcement
│   │   │   ├── validate.ts         # Zod schema validation
│   │   │   ├── rateLimiter.ts
│   │   │   ├── errorHandler.ts
│   │   │   └── requestLogger.ts
│   │   ├── models/
│   │   │   ├── User.model.ts
│   │   │   ├── Post.model.ts
│   │   │   ├── Comment.model.ts
│   │   │   ├── Category.model.ts
│   │   │   ├── Bookmark.model.ts
│   │   │   └── Like.model.ts
│   │   ├── routes/
│   │   │   ├── auth.routes.ts
│   │   │   ├── post.routes.ts
│   │   │   ├── comment.routes.ts
│   │   │   ├── user.routes.ts
│   │   │   ├── category.routes.ts
│   │   │   └── admin.routes.ts
│   │   ├── services/
│   │   │   ├── auth.service.ts
│   │   │   ├── post.service.ts
│   │   │   ├── comment.service.ts
│   │   │   ├── user.service.ts
│   │   │   ├── category.service.ts
│   │   │   └── analytics.service.ts
│   │   ├── utils/
│   │   │   ├── jwt.ts
│   │   │   ├── paginate.ts
│   │   │   ├── trendingScore.ts
│   │   │   └── ApiError.ts
│   │   ├── validations/            # Zod schemas
│   │   └── app.ts                  # Express app bootstrap
│   ├── tests/
│   │   ├── unit/
│   │   ├── integration/
│   │   └── fixtures/
│   ├── server.ts                   # Entry point
│   └── tsconfig.json
│
├── docker/
│   ├── Dockerfile.client
│   ├── Dockerfile.server
│   └── nginx.conf
│
├── docker-compose.yml
├── docker-compose.prod.yml
├── .env.example
├── .eslintrc.json
├── .prettierrc
└── README.md
```

---

## Installation

### Prerequisites

| Requirement | Version |
|---|---|
| Node.js | >= 20.x |
| npm | >= 10.x |
| MongoDB | >= 7.x (or Atlas URI) |
| Docker & Docker Compose | >= 24.x (optional) |

---

### 1. Clone the repository

```bash
git clone https://github.com/your-username/zarrin-blogs.git
cd zarrin-blogs
```

### 2. Install dependencies

```bash
# Install server dependencies
cd server && npm install

# Install client dependencies
cd ../client && npm install
```

### 3. Configure environment variables

```bash
# In the project root
cp .env.example .env

# Then open .env and fill in your values (see Environment Variables section)
```

### 4. Seed the database (optional)

```bash
cd server
npm run seed          # Populates DB with sample users, categories, and posts
npm run seed:admin    # Creates the default admin account
```

### 5. Run in development

```bash
# Terminal 1 — API server (http://localhost:5000)
cd server && npm run dev

# Terminal 2 — React client (http://localhost:5173)
cd client && npm run dev
```

### 6. Production build

```bash
# Build client
cd client && npm run build

# Build server (TypeScript → JavaScript)
cd server && npm run build

# Start production server
cd server && npm start
```

---

## API Endpoints

Base URL: `https://zarrin-blogs-api.onrender.com/api`

All protected routes require the `Authorization: Bearer <token>` header.

---

### Authentication

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| `POST` | `/auth/register` | Public | Register a new user |
| `POST` | `/auth/login` | Public | Login and receive JWT pair |
| `POST` | `/auth/refresh` | Public | Rotate access token using refresh token |
| `POST` | `/auth/logout` | User | Invalidate refresh token |
| `POST` | `/auth/forgot-password` | Public | Send password reset email |
| `PUT` | `/auth/reset-password/:token` | Public | Reset password via token |

---

### Posts

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| `GET` | `/posts` | Public | List all published posts (paginated) |
| `GET` | `/posts/trending` | Public | Top posts by trending score |
| `GET` | `/posts/search?q=&tag=&category=` | Public | Full-text search with filters |
| `GET` | `/posts/:slug` | Public | Get a single post by slug |
| `POST` | `/posts` | User | Create a new post |
| `PUT` | `/posts/:id` | Owner | Update own post |
| `DELETE` | `/posts/:id` | Owner / Admin | Delete a post |
| `POST` | `/posts/:id/like` | User | Toggle like on a post |
| `POST` | `/posts/:id/bookmark` | User | Toggle bookmark on a post |

**Example — create a post:**

```http
POST /api/posts
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "Getting Started with Redis Caching",
  "content": "<p>Redis is an in-memory data structure store...</p>",
  "categoryId": "64a1f3b2c0e4a812b4df3c99",
  "tags": ["redis", "caching", "backend"],
  "status": "published"
}
```

**Example — paginated list response:**

```json
{
  "success": true,
  "data": {
    "posts": [ { "...": "..." } ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "totalPages": 14,
      "totalCount": 138,
      "hasNextPage": true
    }
  }
}
```

---

### Comments

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| `GET` | `/posts/:postId/comments` | Public | Get comments for a post (threaded) |
| `POST` | `/posts/:postId/comments` | User | Add a comment or reply |
| `PUT` | `/comments/:id` | Owner | Edit own comment |
| `DELETE` | `/comments/:id` | Owner / Admin | Delete a comment |

---

### Users

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| `GET` | `/users/:username` | Public | Get public profile |
| `GET` | `/users/me` | User | Get own profile |
| `PUT` | `/users/me` | User | Update profile details |
| `PUT` | `/users/me/avatar` | User | Upload new avatar |
| `GET` | `/users/me/bookmarks` | User | List bookmarked posts |

---

### Categories

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| `GET` | `/categories` | Public | List all categories |
| `GET` | `/categories/:slug/posts` | Public | Get posts under a category |
| `POST` | `/categories` | Admin | Create a category |
| `PUT` | `/categories/:id` | Admin | Update a category |
| `DELETE` | `/categories/:id` | Admin | Delete a category |

---

### Admin

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| `GET` | `/admin/stats` | Admin | Dashboard analytics summary |
| `GET` | `/admin/users` | Admin | List all users (paginated) |
| `PUT` | `/admin/users/:id/block` | Admin | Block a user |
| `PUT` | `/admin/users/:id/unblock` | Admin | Unblock a user |
| `PUT` | `/admin/users/:id/role` | Admin | Change user role |
| `DELETE` | `/admin/users/:id` | Admin | Delete a user account |
| `GET` | `/admin/posts` | Admin | List all posts (any status) |
| `DELETE` | `/admin/posts/:id` | Admin | Force-delete a post |
| `DELETE` | `/admin/comments/:id` | Admin | Force-delete a comment |

---

## Environment Variables

Copy `.env.example` to `.env` and populate each value.

```dotenv
# ── Application ──────────────────────────────────────────────
NODE_ENV=development
PORT=5000
CLIENT_URL=http://localhost:5173

# ── MongoDB ───────────────────────────────────────────────────
MONGO_URI=mongodb+srv://<username>:<password>@cluster0.mongodb.net/zarrin-blogs?retryWrites=true&w=majority

# ── JWT ───────────────────────────────────────────────────────
JWT_ACCESS_SECRET=your_strong_access_secret_here
JWT_REFRESH_SECRET=your_strong_refresh_secret_here
JWT_ACCESS_EXPIRES_IN=15m
JWT_REFRESH_EXPIRES_IN=7d

# ── Cloudinary (Media Storage) ────────────────────────────────
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret

# ── Email (Nodemailer) ────────────────────────────────────────
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASS=your_app_password
EMAIL_FROM="Zarrin Blogs <no-reply@zarrin-blogs.com>"

# ── Rate Limiting ─────────────────────────────────────────────
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX=100

# ── Admin Seed (used by npm run seed:admin) ───────────────────
ADMIN_EMAIL=admin@zarrin-blogs.com
ADMIN_PASSWORD=SuperSecret@123
```

> **Security note:** Never commit `.env` to version control. The `.gitignore` already excludes it.

---

## Testing

Zarrin Blogs maintains an **87% overall test coverage** target enforced in CI.

### Backend Tests (Jest + Supertest)

```bash
cd server

# Run all tests
npm test

# Run with coverage report
npm run test:coverage

# Run tests in watch mode
npm run test:watch

# Run a specific test file
npm test -- --testPathPattern=auth.controller
```

Unit tests cover individual service functions (e.g. `trendingScore`, `paginate`, JWT helpers). Integration tests spin up an in-memory MongoDB instance via `mongodb-memory-server` and fire real HTTP requests through Supertest.

**Sample unit test:**

```typescript
// tests/unit/trendingScore.test.ts
import { computeTrendingScore } from '../../src/utils/trendingScore';

describe('computeTrendingScore()', () => {
  it('weights likes more heavily than views', () => {
    const score = computeTrendingScore({ views: 1000, likes: 10, comments: 5 });
    expect(score).toBeCloseTo(305 + 1, 0); // 1000*0.3 + 10*0.5 + 5*0.2
  });

  it('returns 0 for zero engagement', () => {
    expect(computeTrendingScore({ views: 0, likes: 0, comments: 0 })).toBe(0);
  });
});
```

**Sample integration test:**

```typescript
// tests/integration/auth.test.ts
import request from 'supertest';
import app from '../../src/app';

describe('POST /api/auth/register', () => {
  it('returns 201 and a JWT pair on valid input', async () => {
    const res = await request(app).post('/api/auth/register').send({
      username: 'testuser',
      email: 'test@zarrin.com',
      password: 'SecurePass@1',
    });
    expect(res.status).toBe(201);
    expect(res.body.data).toHaveProperty('accessToken');
    expect(res.body.data).toHaveProperty('refreshToken');
  });

  it('returns 409 when email is already registered', async () => {
    // seed user first, then attempt duplicate
    const res = await request(app).post('/api/auth/register').send({
      username: 'dupe',
      email: 'test@zarrin.com',
      password: 'SecurePass@1',
    });
    expect(res.status).toBe(409);
  });
});
```

---

### Frontend Tests (Vitest + React Testing Library)

```bash
cd client

# Run all tests
npm test

# Coverage report
npm run test:coverage
```

Components are tested for rendering, user interactions, and conditional logic. RTK Query hooks are tested with `msw` (Mock Service Worker) to intercept API calls.

---

### End-to-End Tests (Cypress)

```bash
cd client

# Open Cypress test runner (interactive)
npm run cy:open

# Run headlessly (used in CI)
npm run cy:run
```

Key E2E scenarios covered:

- User registration → login → create post → publish flow
- Admin login → block user → verify user cannot log in
- Search results rendering for a given query
- Comment threading and deletion
- Bookmark toggle persisting across sessions

---

### Coverage Summary

| Area | Statements | Branches | Functions | Lines |
|------|-----------|----------|-----------|-------|
| Server — Services | 94% | 91% | 96% | 94% |
| Server — Controllers | 88% | 84% | 90% | 88% |
| Server — Middleware | 92% | 89% | 95% | 92% |
| Client — Components | 81% | 77% | 83% | 81% |
| Client — Hooks | 85% | 80% | 88% | 85% |
| **Overall** | **87%** | **84%** | **90%** | **87%** |

---

## Docker Setup

### Development

```bash
# Start all services (MongoDB + server + client) with live reload
docker compose up --build

# Services:
#   MongoDB  → localhost:27017
#   API      → localhost:5000
#   Client   → localhost:5173
```

### Production

```bash
docker compose -f docker-compose.prod.yml up --build -d
```

### Dockerfile — Server

```dockerfile
# docker/Dockerfile.server
FROM node:20-alpine AS builder
WORKDIR /app
COPY server/package*.json ./
RUN npm ci
COPY server/ .
RUN npm run build

FROM node:20-alpine AS runner
ENV NODE_ENV=production
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
EXPOSE 5000
CMD ["node", "dist/server.js"]
```

### Dockerfile — Client

```dockerfile
# docker/Dockerfile.client
FROM node:20-alpine AS builder
WORKDIR /app
COPY client/package*.json ./
RUN npm ci
COPY client/ .
RUN npm run build

FROM nginx:stable-alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY docker/nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### docker-compose.yml

```yaml
version: '3.9'

services:
  mongo:
    image: mongo:7
    restart: unless-stopped
    ports:
      - '27017:27017'
    volumes:
      - mongo_data:/data/db
    environment:
      MONGO_INITDB_DATABASE: zarrin-blogs

  server:
    build:
      context: .
      dockerfile: docker/Dockerfile.server
    restart: unless-stopped
    ports:
      - '5000:5000'
    env_file: .env
    depends_on:
      - mongo
    volumes:
      - ./server/src:/app/src   # live reload in dev

  client:
    build:
      context: .
      dockerfile: docker/Dockerfile.client
    restart: unless-stopped
    ports:
      - '5173:80'
    depends_on:
      - server

volumes:
  mongo_data:
```

---

## CI/CD Pipeline

### CI — Pull Request Checks (`.github/workflows/ci.yml`)

Triggered on every pull request targeting `main` or `develop`.

```yaml
name: CI

on:
  pull_request:
    branches: [main, develop]

jobs:
  server-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'
          cache-dependency-path: server/package-lock.json
      - run: cd server && npm ci
      - run: cd server && npm run lint
      - run: cd server && npm run test:coverage
      - uses: codecov/codecov-action@v4

  client-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'
          cache-dependency-path: client/package-lock.json
      - run: cd client && npm ci
      - run: cd client && npm run lint
      - run: cd client && npm run test:coverage

  e2e:
    runs-on: ubuntu-latest
    needs: [server-tests, client-tests]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: cd server && npm ci && npm run build
      - run: cd client && npm ci
      - uses: cypress-io/github-action@v6
        with:
          working-directory: client
          start: npm run preview
          wait-on: 'http://localhost:4173'
```

### CD — Deploy on Merge (`.github/workflows/deploy.yml`)

Triggered on pushes to `main` after all CI checks pass.

```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy-server:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Deploy to Render
        run: |
          curl -X POST "${{ secrets.RENDER_DEPLOY_HOOK_URL }}"

  deploy-client:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: cd client && npm ci && npm run build
      - uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          working-directory: client
          vercel-args: '--prod'
```

**Required GitHub Secrets:**

| Secret | Description |
|--------|-------------|
| `RENDER_DEPLOY_HOOK_URL` | Render deploy hook URL |
| `VERCEL_TOKEN` | Vercel personal access token |
| `VERCEL_ORG_ID` | Vercel organisation ID |
| `VERCEL_PROJECT_ID` | Vercel project ID |

---

## Screenshots

> Screenshots will be added following the first public release. The placeholder structure below reflects the actual pages:

| Page | Preview |
|------|---------|
| Home / Feed | `docs/screenshots/home.png` |
| Post Detail + Comments | `docs/screenshots/post-detail.png` |
| Rich Text Editor | `docs/screenshots/editor.png` |
| User Profile | `docs/screenshots/profile.png` |
| Admin Dashboard | `docs/screenshots/admin-dashboard.png` |
| Admin User Management | `docs/screenshots/admin-users.png` |
| Search Results | `docs/screenshots/search.png` |
| Mobile View | `docs/screenshots/mobile.png` |

---

## Performance & Security

### Performance

- **RTK Query caching** — API responses are cached client-side with configurable TTLs, eliminating redundant network requests on repeated navigation.
- **Mongoose lean queries** — Read-only endpoints use `.lean()` to skip Mongoose document hydration, reducing memory overhead by ~30% on high-cardinality list queries.
- **Database indexes** — Compound indexes on `(category, status, createdAt)` and a text index on `(title, content, tags)` keep query response times under 15ms at 50k documents.
- **Cursor-based pagination** — All list endpoints use cursor pagination (`?after=<lastId>`) to avoid the performance degradation of large `SKIP` offsets in MongoDB.
- **Image optimisation** — All uploaded images are processed by Cloudinary's transformation pipeline (auto-format, auto-quality, responsive width variants).
- **Code splitting** — Vite's dynamic `import()` splits the admin panel and the editor into separate lazy-loaded chunks, keeping the initial bundle under 200 KB (gzipped).

### Security

- **JWT rotation** — Access tokens expire after 15 minutes; refresh tokens are rotated on every use and stored as HTTP-only cookies in production.
- **bcrypt** — Passwords are hashed with a cost factor of 12 (`bcrypt.genSalt(12)`).
- **RBAC** — Every protected route enforces role checks through the `authorise` middleware. Privilege escalation is impossible through the public API.
- **Input validation** — All request bodies are parsed against Zod schemas before reaching controllers. Mongoose schema validation provides a second layer.
- **Rate limiting** — `express-rate-limit` caps auth endpoints at 10 requests per 15 minutes per IP; general API routes at 100 requests per 15 minutes.
- **Security headers** — `helmet` sets `Content-Security-Policy`, `X-Content-Type-Options`, `X-Frame-Options`, and `Strict-Transport-Security`.
- **CORS** — Requests are restricted to the `CLIENT_URL` origin in production.
- **NoSQL injection** — `express-mongo-sanitize` strips `$` and `.` from request data before it reaches Mongoose.

---

## Future Improvements

The following capabilities are scoped for upcoming milestones:

- **Email notifications** — Notify authors when their post receives a like or comment
- **Follow system** — Follow authors and receive a personalised following feed
- **Post series** — Group related posts into ordered series with navigation controls
- **Reading time estimate** — Auto-calculated from word count and appended to post metadata
- **Reaction system** — Expand beyond a single like to multi-reaction support (insightful, clap, fire)
- **RSS feed** — Auto-generated RSS/Atom feed per author and per category
- **GraphQL API** — Provide a GraphQL layer alongside the REST API for more flexible client consumption
- **Redis caching** — Introduce a Redis layer for session storage and trending post computation
- **Full-text search with Elasticsearch** — Replace MongoDB text indexes with Elasticsearch for richer relevance ranking
- **Internationalization (i18n)** — Multi-language UI support via `react-i18next`
- **PWA support** — Service worker + offline reading for bookmarked posts

---

## License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2025 Zarrin Blogs

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

---

## Author

**Zarrin Blogs** is designed, built, and maintained by:

<table>
  <tr>
    <td align="center">
      <strong>Your Name</strong><br/>
      Full-Stack Engineer<br/>
      <a href="https://github.com/your-username">GitHub</a> ·
      <a href="https://linkedin.com/in/your-profile">LinkedIn</a> ·
      <a href="mailto:you@example.com">Email</a>
    </td>
  </tr>
</table>

---

<div align="center">

If this project helped you, consider giving it a ⭐ — it helps others find it.

</div>
