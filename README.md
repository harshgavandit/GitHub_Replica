# GitHub Replica

A full-stack GitHub-inspired application built with the MERN stack, plus a custom lightweight version-control workflow implemented on the backend. The project includes user authentication, repository management, issue management, profile pages, and a simple commit/push/pull/revert CLI flow backed by Amazon S3.

## Live Demo

https://github-replica.onrender.com

## Overview

This repository is split into two applications:

- **`frontend-main`**: a React + Vite client that provides the GitHub-like user interface.
- **`backend-main`**: an Express + MongoDB API server that also exposes a custom repository CLI through `yargs`.

## Features

### Web application

- User signup and login with JWT-based authentication
- Profile page with GitHub-style layout and activity heat map
- Repository listing for the current user
- Suggested repository list
- Repository CRUD and visibility toggle endpoints
- Issue CRUD endpoints
- Socket.io room joining support

### Custom version-control workflow

The backend also includes a lightweight local repository flow using a hidden `.apnaGit` folder:

- `init` initializes repository metadata
- `add <file>` stages a file
- `commit <message>` creates a commit snapshot
- `push` uploads commit snapshots to S3
- `pull` downloads commit snapshots from S3
- `revert <commitID>` restores files from a selected commit

## Tech Stack

### Frontend

- React 18
- Vite
- React Router
- Primer React
- Axios
- Vitest
- ESLint

### Backend

- Node.js
- Express
- MongoDB / Mongoose
- JWT
- bcryptjs
- Socket.io
- AWS S3 SDK
- yargs

## Project Structure

```text
GitHub_Replica/
├── README.md
├── backend-main/
│   ├── config/
│   ├── controllers/
│   ├── middleware/
│   ├── models/
│   ├── routes/
│   ├── index.js
│   └── package.json
└── frontend-main/
    ├── src/
    │   ├── components/
    │   ├── App.jsx
    │   ├── Routes.jsx
    │   ├── authContext.jsx
    │   └── main.jsx
    └── package.json
```

## Getting Started

### Prerequisites

- Node.js 18+
- npm
- MongoDB connection string
- AWS credentials and an S3 bucket if you want to use the custom push/pull CLI flow

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/harshgavandit/GitHub_Replica.git
cd GitHub_Replica
```

### 2. Install frontend dependencies

```bash
cd frontend-main
npm install
```

### 3. Install backend dependencies

```bash
cd ../backend-main
npm install
```

## Environment Setup

Create a `.env` file inside `backend-main` with the following values:

```env
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET_KEY=your_jwt_secret
PORT=3002
S3_BUCKET=your_s3_bucket_name
```

### Notes

- The frontend currently calls the backend at `http://localhost:3002`, so running the backend on port `3002` is recommended for local development.
- The custom S3-backed CLI also reads bucket configuration from `backend-main/config/aws-config.js`. Update that file with your actual bucket details before using `push` or `pull`.

## Running the Project Locally

### Start the backend

```bash
cd backend-main
npm start
```

### Start the frontend

```bash
cd frontend-main
npm run dev
```

The frontend will usually be available at `http://localhost:5173`.

## Available Frontend Scripts

From `frontend-main`:

```bash
npm run dev
npm run build
npm run lint
npm test -- --run
```

## Backend CLI Commands

From `backend-main`:

```bash
node index.js start
node index.js init
node index.js add path/to/file
node index.js commit "commit message"
node index.js push
node index.js pull
node index.js revert <commitID>
```

## API Overview

### User routes

- `POST /signup`
- `POST /login`
- `GET /allUsers`
- `GET /userProfile/:id`
- `PUT /updateProfile/:id`
- `DELETE /deleteProfile/:id`

### Repository routes

- `POST /repo/create`
- `GET /repo/all`
- `GET /repo/:id`
- `GET /repo/name/:name`
- `GET /repo/user/:userID`
- `PUT /repo/update/:id`
- `PATCH /repo/toggle/:id`
- `DELETE /repo/delete/:id`

### Issue routes

- `POST /issue/create`
- `PUT /issue/update/:id`
- `DELETE /issue/delete/:id`
- `GET /issue/all`
- `GET /issue/:id`

## Current UI Pages

- `/` - dashboard
- `/auth` - login page
- `/signup` - registration page
- `/profile` - user profile

## Validation Status

Current repository commands observed during verification:

- `npm run build` in `frontend-main` succeeds
- `npm run lint` in `frontend-main` currently fails because of existing ESLint issues in the source files
- `npm test -- --run` in `frontend-main` exits because no test files are present

## Future Improvements

- Replace hardcoded API base URLs with environment-based configuration
- Add frontend and backend automated tests
- Add repository creation/editing UI flows
- Improve issue-to-repository linking in the API
- Harden auth, validation, and error handling paths

## License

This project is currently unlicensed in the repository metadata.
