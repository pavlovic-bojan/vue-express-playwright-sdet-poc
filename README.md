# Student Management System

A comprehensive **multi-tenant Student Management System (SMS)** designed for universities, colleges, and schools. SMS digitizes and simplifies administrative and academic operations including tenant management, student lifecycle, programs, courses, exams, finance, records, ticketing, and notifications.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Project Structure](#project-structure)
- [Quick Start](#quick-start)
- [Documentation](#documentation)
- [Tech Stack](#tech-stack)
- [Development](#development)
- [Testing](#testing)
- [Seed Users](#seed-users)
- [Live App & Test Reports](#live-app--test-reports)
- [GitHub Actions (CI)](#github-actions-ci)
- [License](#license)

## 🎯 Overview

**SMS** is a modern, full-stack application that provides:

- **Multi-Tenancy**: Manage multiple educational institutions (tenants) with data isolation
- **Tenant Management**: Create, update, and deactivate institutions (Platform Admin only)
- **User Management**: Role-based users (Platform Admin, School Admin, Professor, Student)
- **Student Lifecycle**: Enrollments, programs, courses, exams, finance, transcripts
- **Ticketing System**: Bug reports with status and priority tracking
- **Notifications**: User-action and ticket notifications
- **Real-time Dashboard**: Analytics and overview for admins

### Roles

| Role | Responsibilities |
|------|------------------|
| **Platform Admin** | Tenant management, system oversight |
| **School Admin** | CRUD for students, programs, courses, finance |
| **Professor** | Exam grading, course assignments |
| **Student** | Enrollment, exam registration, transcript access |

## ✨ Features

### Core Features

- ✅ **JWT Authentication** (login, register, forgot password)
- ✅ **Role-based Access Control** (Platform Admin, School Admin, Professor, Student)
- ✅ **Multi-Tenant Architecture** with tenant isolation
- ✅ **Tenant CRUD** (Platform Admin only)
- ✅ **User Management** (list, create, edit, suspend, delete)
- ✅ **Student Management** with enrollments (many-to-many with tenants)
- ✅ **Programs & Courses** CRUD
- ✅ **Exams** (periods, terms, registrations)
- ✅ **Finance** (tuitions, payments)
- ✅ **Records** (transcripts)
- ✅ **Bug Reports (Tickets)** with status and priority
- ✅ **Notifications** (list, mark-read)
- ✅ **Dashboard** with tenant context
- ✅ **Internationalization** – English, Serbian (Latin), Serbian (Cyrillic)
- ✅ **Dark Mode** support

### Technical Features

- ✅ **TypeScript** – Full type safety across backend and frontend
- ✅ **RESTful API** with Swagger/OpenAPI documentation
- ✅ **PostgreSQL Database** with Prisma ORM
- ✅ **Responsive Design** – Quasar components, Tailwind CSS
- ✅ **Comprehensive Testing** – Unit, Integration, API, E2E, Performance
- ✅ **Allure Reporting** – Integrated test reports
- ✅ **k6 Performance Tests** – Smoke, load, stress, spike, soak

## 📁 Project Structure

```
sms/
├── backend/              # Backend API (Node.js + Express + Prisma)
│   ├── src/
│   │   ├── modules/      # Feature modules (auth, tenant, students, etc.)
│   │   ├── middleware/   # Express middleware (auth, tenant context, error)
│   │   ├── config/       # Configuration
│   │   └── __tests__/    # Unit and integration tests
│   ├── prisma/           # Database schema, migrations, seed
│   └── README.md         # Backend documentation
│
├── frontend/             # Frontend Application (Vue 3 + Quasar + Vite)
│   ├── src/
│   │   ├── pages/        # Page components
│   │   ├── components/   # Reusable components
│   │   ├── api/          # API service layer
│   │   ├── router/       # Vue Router + guards
│   │   ├── stores/       # Pinia stores
│   │   └── locales/      # i18n translation files
│   ├── public/           # Static assets
│   └── README.md         # Frontend documentation
│
├── tests/                # Test Suites
│   ├── e2e/              # Playwright (API + E2E tests)
│   ├── performance/      # k6 performance tests
│   └── README.md         # Tests documentation
│
├── docs/                 # Project Documentation
│   ├── BRD.md            # Business Requirements Document
│   ├── PRD.md            # Product Requirements Document
│   ├── HLD.md            # High-Level Design
│   ├── TDD.md            # Test-Driven Development
│   └── OpenAPI.yaml      # API specification
│
├── images/               # Screenshots (auth page, admin page)
└── README.md             # This file
```

## 🚀 Quick Start

### Option A: Docker (Recommended for Local Development)

**Prerequisites:** Docker Desktop

1. **Create `.env` in `backend/`** (copy from `backend/.env.example`):
   ```env
   DATABASE_URL="postgresql://sms:sms_secret@postgres:5432/sms_db"
   DIRECT_URL="postgresql://sms:sms_secret@postgres:5432/sms_db"
   JWT_SECRET="your-super-secret-jwt-key-minimum-32-characters"
   SESSION_SECRET="your-session-secret-key-minimum-32-characters"
   ```

2. **Start all services:**
   ```bash
   docker compose up -d
   ```

3. **Run migrations and seed** (first time only):
   ```bash
   docker compose exec backend npx prisma migrate deploy
   docker compose exec backend npm run prisma:seed
   ```

4. **Access the application:**
   - Frontend: http://localhost:80 (or port shown by Docker)
   - Backend API: http://localhost:4000
   - Swagger docs: http://localhost:4000/api-docs

### Option B: Local Setup (without Docker)

**Prerequisites:**
- **Node.js** >= 20.0.0
- **npm** >= 9.0.0
- **PostgreSQL** >= 14.0
- **k6** (for performance tests only)

#### Installation

This project uses **npm workspaces** for monorepo management. Install all dependencies:

```bash
npm install
```

#### Backend Setup (Local)

1. **Create `.env` file in `backend/` directory:**
   ```env
   DATABASE_URL="postgresql://USER:PASSWORD@localhost:5432/sms_db"
   DIRECT_URL="postgresql://USER:PASSWORD@localhost:5432/sms_db"
   JWT_SECRET="your-super-secret-jwt-key-minimum-32-characters"
   SESSION_SECRET="your-session-secret-key-minimum-32-characters"
   PORT=4000
   NODE_ENV=development
   ```

2. **Generate Prisma client and run migrations:**
   ```bash
   cd backend
   npx prisma generate
   npx prisma migrate deploy
   npm run prisma:seed
   cd ..
   ```

3. **Start backend server:**
   ```bash
   npm run backend:dev
   ```

   Backend will be available at `http://localhost:4000`
   - API base path: `/api` (e.g. `/api/health`, `/api/auth/login`, `/api/tenants`)
   - Swagger UI: `http://localhost:4000/api-docs`

**📖 [Read Full Backend Documentation →](./backend/README.md)**

#### Frontend Setup (Local)

1. **Create `.env` file in `frontend/` directory:**
   ```env
   VITE_API_URL=http://localhost:4000/api
   ```

2. **Start development server:**
   ```bash
   npm run frontend:dev
   ```

   Frontend will be available at `http://localhost:5173`

**📖 [Read Full Frontend Documentation →](./frontend/README.md)**

#### Tests Setup

**With Docker:** Backend and DB must be running (`docker compose up -d`). Run tests from host:
```bash
npm run tests:api         # API_URL=http://localhost:4000
npm run tests:e2e         # Frontend + backend required
npm run tests:performance # BASE_URL=http://localhost:4000
```

**Without Docker:** Test dependencies are installed when you run `npm install` from root.

**Run tests from root directory:**
```bash
# API tests (backend required)
npm run tests:api

# E2E tests (backend + frontend required)
npm run tests:e2e

# Performance tests (backend required)
npm run tests:performance

# All tests
npm run test:all
```

**📖 [Read Full Tests Documentation →](./tests/README.md)**

## 📚 Documentation

### Main Documentation

- **[Backend Documentation](./backend/README.md)** – API, setup, endpoints, authentication
- **[Frontend Documentation](./frontend/README.md)** – Setup, components, styling, i18n
- **[Tests Documentation](./tests/README.md)** – Playwright (API + E2E), k6 performance

### Project Documentation

- **[Business Requirements Document](./docs/BRD.md)** – Complete BRD and features
- **[Product Requirements Document](./docs/PRD.md)** – PRD
- **[High-Level Design](./docs/HLD.md)** – Architecture
- **[Test-Driven Development](./docs/TDD.md)** – TDD guide

## 🛠️ Tech Stack

### Backend

- **Runtime**: Node.js
- **Framework**: Express.js
- **Language**: TypeScript
- **ORM**: Prisma
- **Database**: PostgreSQL
- **Authentication**: JWT (jsonwebtoken)
- **Validation**: Express Validator
- **Documentation**: Swagger/OpenAPI
- **Testing**: Vitest, Supertest
- **Logging**: Winston

### Frontend

- **Framework**: Vue 3 (Composition API)
- **UI Framework**: Quasar
- **Language**: TypeScript
- **Build**: Vite
- **Styling**: Tailwind CSS, Quasar SASS
- **State Management**: Pinia
- **HTTP Client**: Axios
- **i18n**: vue-i18n
- **Testing**: Vitest, Vue Test Utils

### Testing

- **E2E**: Playwright + TypeScript (API + E2E, POM)
- **Performance**: k6 (smoke, load, stress, spike, soak)
- **Reporting**: Allure

## 💻 Development

### Running Development Servers

From root directory:

```bash
# Terminal 1 – Backend
npm run backend:dev

# Terminal 2 – Frontend
npm run frontend:dev
```

### Available Scripts

From root directory:

```bash
# Backend
npm run backend:dev              # Run backend in development
npm run backend:test             # Run backend tests (unit + integration)
npm run backend:test:unit        # Unit tests only (no DB)
npm run backend:test:integration # Integration tests only (requires DB)

# Frontend
npm run frontend:dev             # Run frontend in development
npm run frontend:test            # Run frontend tests

# Tests
npm run tests:api                # Playwright API tests
npm run tests:e2e                # Playwright E2E tests
npm run tests:api:report         # Allure report (API)
npm run tests:e2e:report         # Allure report (E2E)
npm run tests:performance        # k6 performance tests

# All
npm run test:all                 # All tests (backend + frontend + api + e2e + perf)
npm run lint                     # Lint backend and frontend
```

From `backend/`:
```bash
npm run prisma:seed              # Seed database with test data
npm run prisma:migrate           # Interactive migrations
```

## 🧪 Testing

### Test Coverage

- ✅ **Backend**: Unit tests + Integration tests (Vitest)
- ✅ **Frontend**: Unit tests (stores, router guards, components)
- ✅ **API**: Playwright API tests with JSON schema validation
- ✅ **E2E**: Playwright E2E tests with Page Object Model
- ✅ **Performance**: k6 (smoke, baseline, load, stress, spike, soak)

### Running Tests

```bash
# Unit tests (no DB required)
npm run backend:test:unit
npm run frontend:test

# Full test suite (requires PostgreSQL, backend, frontend for E2E)
npm run test:all
```

## 👤 Seed Users

After running `npm run prisma:seed` in `backend/` (or `docker compose exec backend npm run prisma:seed`):

**Change passwords in production.** Seed does not create STUDENT login accounts; students exist as records but do not have user logins by default.

Use these users to log in and test the app (e.g. at the Vercel frontend URL):

| Username (email) | Password | Role |
|------------------|----------|------|
| platform-admin@sms.edu | seed-platform-admin-change-me | PLATFORM_ADMIN |
| admin@bg.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@ns.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@ni.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@kg.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@bl.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@sa.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@zg.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@lj.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@sk.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@cg.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@pg.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@pr.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@tz.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@mo.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@su.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@np.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@ca.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@le.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@sm.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@zr.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@pa.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@sh.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@kr.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@vr.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@uz.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@va.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@so.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@ki.edu | seed-admin-change-me | SCHOOL_ADMIN |
| admin@ja.edu | seed-admin-change-me | SCHOOL_ADMIN |
| prof1@bg.edu … prof8@bg.edu | seed-prof-change-me | PROFESSOR |
| prof1@ns.edu … prof8@ns.edu | seed-prof-change-me | PROFESSOR |
| prof1@ni.edu … prof8@ni.edu | seed-prof-change-me | PROFESSOR |
| prof1@kg.edu … prof8@kg.edu | seed-prof-change-me | PROFESSOR |
| prof1@bl.edu … prof8@bl.edu | seed-prof-change-me | PROFESSOR |
| prof1@sa.edu … prof8@sa.edu | seed-prof-change-me | PROFESSOR |
| prof1@zg.edu … prof8@zg.edu | seed-prof-change-me | PROFESSOR |
| prof1@lj.edu … prof8@lj.edu | seed-prof-change-me | PROFESSOR |
| prof1@sk.edu … prof8@sk.edu | seed-prof-change-me | PROFESSOR |
| prof1@cg.edu … prof8@cg.edu | seed-prof-change-me | PROFESSOR |
| prof1@pg.edu … prof8@pg.edu | seed-prof-change-me | PROFESSOR |
| prof1@pr.edu … prof8@pr.edu | seed-prof-change-me | PROFESSOR |
| prof1@tz.edu … prof8@tz.edu | seed-prof-change-me | PROFESSOR |
| prof1@mo.edu … prof8@mo.edu | seed-prof-change-me | PROFESSOR |
| prof1@su.edu … prof8@su.edu | seed-prof-change-me | PROFESSOR |
| prof1@np.edu … prof8@np.edu | seed-prof-change-me | PROFESSOR |
| prof1@ca.edu … prof8@ca.edu | seed-prof-change-me | PROFESSOR |
| prof1@le.edu … prof8@le.edu | seed-prof-change-me | PROFESSOR |
| prof1@sm.edu … prof8@sm.edu | seed-prof-change-me | PROFESSOR |
| prof1@zr.edu … prof8@zr.edu | seed-prof-change-me | PROFESSOR |
| prof1@pa.edu … prof8@pa.edu | seed-prof-change-me | PROFESSOR |
| prof1@sh.edu … prof8@sh.edu | seed-prof-change-me | PROFESSOR |
| prof1@kr.edu … prof8@kr.edu | seed-prof-change-me | PROFESSOR |
| prof1@vr.edu … prof8@vr.edu | seed-prof-change-me | PROFESSOR |
| prof1@uz.edu … prof8@uz.edu | seed-prof-change-me | PROFESSOR |
| prof1@va.edu … prof8@va.edu | seed-prof-change-me | PROFESSOR |
| prof1@so.edu … prof8@so.edu | seed-prof-change-me | PROFESSOR |
| prof1@ki.edu … prof8@ki.edu | seed-prof-change-me | PROFESSOR |
| prof1@ja.edu … prof8@ja.edu | seed-prof-change-me | PROFESSOR |

## 🌐 Live App & Test Reports

### Live Application

| Component | URL |
|-----------|-----|
| **Frontend** (Vercel) | [https://student-management-system-frontend-topaz.vercel.app/](https://student-management-system-frontend-topaz.vercel.app/) |
| **Backend API** | [https://student-management-system-backend-gwy8.onrender.com](https://student-management-system-backend-gwy8.onrender.com) |
| **Swagger UI** | [https://student-management-system-backend-gwy8.onrender.com/api-docs/](https://student-management-system-backend-gwy8.onrender.com/api-docs/) |

API base path: `/api` (e.g. `/api/health`, `/api/auth/login`, `/api/tenants`).

### Test Reports (GitHub Pages)

| Report | URL |
|--------|-----|
| **Landing** | [https://pavlovic-bojan.github.io/Student-Management-System/](https://pavlovic-bojan.github.io/Student-Management-System/) |
| **Allure** (Playwright API + E2E) | [allure/](https://pavlovic-bojan.github.io/Student-Management-System/allure/) |
| **k6 Load Test** | [load/](https://pavlovic-bojan.github.io/Student-Management-System/load/) |

Deployed after each Playwright run and Performance workflow. Enable **Settings → Pages → Deploy from branch** → `gh-pages`.

## ⚙️ GitHub Actions (CI)

Workflows: backend deploy (Render), frontend deploy (Vercel), [Playwright tests](.github/workflows/playwright.yml), [performance tests](.github/workflows/performance.yml).

Secrets (set in **Settings → Secrets and variables → Actions**):

| Secret | Used by | Description |
|--------|---------|-------------|
| `BACKEND_URL` | backend-deploy, playwright, performance | Production backend URL |
| `FRONTEND_URL` | playwright | Production frontend URL |
| `TEST_USER_EMAIL` | playwright | `platform-admin@sms.edu` |
| `TEST_USER_PASSWORD` | playwright | `seed-platform-admin-change-me` |
| `DATABASE_URL` | backend-deploy | PostgreSQL connection string |
| `TEST_DATABASE_URL` | backend-deploy | Test DB for integration |
| `JWT_SECRET` | backend-deploy | JWT signing secret |
| `SESSION_SECRET` | backend-deploy | Session secret |
| `RENDER_DEPLOY_HOOK` | backend-deploy | Render webhook URL |
| `PERF_AUTH_TOKEN` | performance (optional) | JWT for auth endpoints |

## 🐳 Docker

Development stack runs via `docker-compose.yml`:

| Service | Port | Description |
|---------|------|-------------|
| postgres | 5432 | PostgreSQL 16 |
| backend | 4000 | Express API |
| frontend | 80 | Vue/Quasar SPA |
| tests | – | Playwright + k6 (run on demand) |

```bash
# Start services
docker compose up -d

# Run tests
docker compose --profile test run --rm tests api
docker compose --profile test run --rm tests e2e
docker compose --profile test run --rm tests performance

# Migrations
docker compose exec backend npx prisma migrate deploy

# Seed
docker compose exec backend npm run prisma:seed
```

## 📄 License

Proprietary License – All rights reserved

## 🔗 Quick Links

- **[Backend README](./backend/README.md)** – Backend API documentation
- **[Frontend README](./frontend/README.md)** – Frontend application documentation
- **[Tests README](./tests/README.md)** – Testing documentation
- **[Business Requirements](./docs/BRD.md)** – Complete BRD

---

**Status**: ✅ Production Ready
