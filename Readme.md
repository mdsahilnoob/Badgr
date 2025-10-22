# Badgr

Badgr is a full‑stack demo application for managing events, registering attendees via CSV upload, generating badges, and minting NFT/POAP style proofs of attendance via the Verbwire NFT API.

This repository contains a Node.js/Express backend and a React + Vite frontend. It demonstrates CSV processing, badge generation (via a Python script), file uploads, email delivery, and integration with an external NFT service (Verbwire). Tests use Jest and an in‑memory MongoDB for quick CI-friendly runs.

## Table of contents
- [What it does](#what-it-does)
- [Tech stack](#tech-stack)
- [Repository layout](#repository-layout)
- [Getting started (local development)](#getting-started-local-development)
  - [Prerequisites](#prerequisites)
  - [Backend](#backend)
  - [Frontend](#frontend)
- [Environment variables](#environment-variables)
- [Testing](#testing)
- [CI / GitHub Actions](#ci--github-actions)
- [Notes & next steps](#notes--next-steps)

## What it does
- Create/manage events and attendees.
- Upload attendees with CSV files and process them into MongoDB.
- Generate attendee badges (Python script under `backend/scripts/generate_badge.py`).
- Upload badge images / metadata and quick‑mint NFTs via Verbwire.
- Send notifications/emails to attendees using Nodemailer.
- Keep a record of mint transactions in the database.

## Tech stack
Backend
- Node.js (Express)
- MongoDB (Mongoose)
- Jest + mongodb-memory-server for tests
- Libraries: axios, multer, csv-parser, nodemailer, form-data
- Python: a small badge generator script (optional integration)

Frontend
- React 18 + Vite
- react-router-dom
- axios for API calls

Dev & CI
- ESLint, nodemon for development
- GitHub Actions (a documented action exists at `.github/actions/mintmeet-ci/README.md`)

## Repository layout
- backend/ — Express app, routes, controllers, models, config, tests, uploads and scripts
- frontend/ — React app (Vite) and services
- .github/actions/mintmeet-ci — README for a reusable CI action (action implementation not included)

Key backend files
- `backend/server.js`, `backend/app.js` — Express server
- `backend/routes/*.js` — API routes
- `backend/controllers/*.js` — Controller logic for events, attendees, minting
- `backend/models/*.js` — Mongoose models (`Event`, `Attendee`, `Transaction`)
- `backend/config/verbwire.js` — Verbwire client config and mock mode detection
- `backend/scripts/generate_badge.py` — Badge generation script

## Getting started (local development)
### Prerequisites
- Node.js 18+ (recommended)
- npm
- Python 3.8+ (only if you want to run the badge generation script)
- MongoDB (optional for tests — tests use an in‑memory server by default)

### Backend
1. Install Node dependencies
   ```powershell
   cd backend
   npm install
   ```
2. (Optional) Install Python deps for badge generator
   ```powershell
   pip install -r scripts/requirements.txt
   ```
3. Start backend in dev mode
   ```powershell
   npm run dev
   ```
4. Backend will use environment variables from `.env` (see [Environment variables](#environment-variables)). If `VERBWIRE_API_KEY` is not set or invalid, the app will run in Verbwire mock mode.

### Frontend
1. Install dependencies
   ```powershell
   cd frontend
   npm install
   ```
2. Start dev server
   ```powershell
   npm run dev
   ```
3. Open the app in your browser at `http://localhost:5173` (or the URL printed by Vite).

## Environment variables
Place these in a `.env` file in `backend/` or provide them in your environment.
- `PORT` — Backend server port (default: `5000`)
- `MONGODB_URI` — MongoDB connection string (default: `mongodb://localhost:27017/eventproof`)
- `FRONTEND_URL` — Frontend origin for CORS (default: `http://localhost:5173`)
- `VERBWIRE_API_KEY` — Verbwire API key. If missing, the code will typically use mock mode for development/test.
- `VERBWIRE_BASE_URL` — (optional) Base URL for Verbwire API.
- `NODE_ENV` — `development` or `production`

## Testing
Backend tests use Jest and an in‑memory MongoDB for fast, isolated runs.

Run backend tests:
```powershell
cd backend
npm test
```

Frontend projects may have lint or test scripts (run from `frontend/`), e.g. `npm run lint` or `npm run build`.

## CI / GitHub Actions
A README for a reusable action is available at `.github/actions/mintmeet-ci/README.md`. It documents inputs like `run-backend`, `run-frontend`, and handling of `VERBWIRE_API_KEY`. The action implementation is not included — you can wire it up by adding an `action.yml` composite action or calling steps directly from `.github/workflows/ci.yml`.

## Notes & next steps
- Add a top‑level CONTRIBUTING.md with setup steps for new developers.
- Provide a `docker-compose.yml` to run MongoDB + backend for easy local setup.
- Add a runnable GitHub Action implementation (`action.yml`) and an example workflow.
- Harden tests for network timeouts when running optional integration tests that use the real Verbwire API.

---
