# Team Task Manager

A full-stack team collaboration and project management application built with **React.js** (frontend) and **Spring Boot** (backend). It provides role-based dashboards, Kanban task tracking, project management, and team oversight — all in a clean, responsive UI.

🔗 **Live Demo:** [team-task-manager-react-js.onrender.com](https://team-task-manager-react-js.onrender.com/dashboard)

> **Demo Credentials (Admin)**
> - Email: `admin@gmail.com`
> - Password: `Admin@123`

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Clone the Repository](#clone-the-repository)
  - [Backend Setup](#backend-setup)
  - [Frontend Setup](#frontend-setup)
  - [Running with Docker](#running-with-docker)
- [API Reference](#api-reference)
- [Environment Variables](#environment-variables)
- [Screenshots](#screenshots)

---

## Overview

Team Task Manager is a modern, production-ready web application designed to help teams organize their work. Admins can create projects, manage members, and monitor overall team progress. Members can view their assigned tasks, update statuses, leave comments, and request to join projects — all within a single, unified platform.

---

## Features

### For Admins
- 📊 **Admin Dashboard** — aggregate stats across all projects, users, and tasks
- 👥 **User Management** — view all users, update roles, and delete accounts
- 📁 **Project Management** — create, update, and delete projects; add or remove members
- ✅ **Full Task Control** — create, assign, update, and delete tasks across all projects
- 🔔 **Join Request Handling** — approve or reject member requests to join projects

### For Members
- 🗂️ **Personal Dashboard** — view your upcoming tasks, overdue items, and recent activity
- 📋 **Task Tracking** — manage tasks across four lifecycle stages: *To-Do → In Progress → In Review → Done*
- 🏷️ **Priority & Due Dates** — tasks include priority levels (Low / Medium / High) and due dates
- 💬 **Comments** — add and delete comments on tasks for team communication
- 🔍 **Project Discovery** — browse projects and submit join requests

### General
- 🔐 **JWT Authentication** — secure login with access/refresh token flow
- 📱 **Responsive UI** — works seamlessly on desktop and mobile
- 🎨 **Minimalist Design** — card-based layout with colored status chips and dynamic hover states
- 🔄 **Kanban Board** — drag-and-drop style task status updates

---

## Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | React 19, Vite, Tailwind CSS v4 |
| **State Management** | Redux Toolkit |
| **Routing** | React Router v7 |
| **Forms & Validation** | React Hook Form + Zod |
| **HTTP Client** | Axios |
| **Notifications** | React Hot Toast |
| **Backend** | Spring Boot 3.5, Java 25 |
| **Security** | Spring Security + JWT |
| **Database** | PostgreSQL |
| **ORM** | Spring Data JPA (Hibernate) |
| **Build Tool** | Maven |
| **Containerization** | Docker (multi-stage builds) |

---

## Repository Structure

This repository uses **Git submodules** to link the frontend and backend as separate repositories.

```
Team_Task_Manager/
├── team_task_manager_react_js/   # React.js frontend (submodule)
├── Team_Task_Manager_Spring/     # Spring Boot backend (submodule)
├── .gitmodules
└── README.md
```

Each submodule has its own detailed README:
- **Frontend:** [`team_task_manager_react_js/README.md`](./team_task_manager_react_js/README.md)
- **Backend:** [`Team_Task_Manager_Spring/README.md`](./Team_Task_Manager_Spring/README.md)

---

## Getting Started

### Prerequisites

Make sure the following are installed:

- [Node.js](https://nodejs.org/) v18+
- [Java](https://adoptium.net/) 21+
- [Maven](https://maven.apache.org/) 3.9+
- [PostgreSQL](https://www.postgresql.org/) 14+
- [Docker](https://www.docker.com/) *(optional, for containerized setup)*

---

### Clone the Repository

This repo uses submodules, so clone it with the `--recurse-submodules` flag:

```bash
git clone --recurse-submodules https://github.com/Ayushyadav14/Team_Task_Manager.git
cd Team_Task_Manager
```

If you've already cloned without the flag, run:

```bash
git submodule update --init --recursive
```

---

### Backend Setup

1. **Navigate to the backend directory:**
   ```bash
   cd Team_Task_Manager_Spring
   ```

2. **Create a PostgreSQL database:**
   ```sql
   CREATE DATABASE team_task_manager;
   ```

3. **Configure environment variables** (see [Environment Variables](#environment-variables)):

   The app reads config from `application.yaml`. You can set them directly or export as environment variables:
   ```bash
   export DB_URL=jdbc:postgresql://localhost:5432/team_task_manager
   export DB_USERNAME=your_db_user
   export DB_PASSWORD=your_db_password
   export JWT_SECRET=your_jwt_secret_key_min_32_chars
   ```

4. **Run the application:**
   ```bash
   ./mvnw spring-boot:run
   ```
   The backend will start on **http://localhost:8080**.

   > On first startup, a default admin account is automatically seeded.

---

### Frontend Setup

1. **Navigate to the frontend directory:**
   ```bash
   cd team_task_manager_react_js
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Set the backend API URL** in a `.env` file:
   ```env
   VITE_API_BASE_URL=http://localhost:8080/api
   ```

4. **Start the development server:**
   ```bash
   npm run dev
   ```
   The frontend will be available at **http://localhost:5173**.

---

### Running with Docker

Both services include a `Dockerfile` with multi-stage builds for production-ready images.

**Backend:**
```bash
cd Team_Task_Manager_Spring
docker build -t team-task-manager-backend .
docker run --rm -p 8080:8080 \
  -e DB_URL=jdbc:postgresql://host.docker.internal:5432/team_task_manager \
  -e DB_USERNAME=your_db_user \
  -e DB_PASSWORD=your_db_password \
  -e JWT_SECRET=your_jwt_secret \
  team-task-manager-backend
```

**Frontend:**
```bash
cd team_task_manager_react_js
docker build -t team-task-manager-frontend .
docker run --rm -p 80:80 team-task-manager-frontend
```

---

## API Reference

All endpoints are prefixed with `/api`. Full details are in the [backend README](./Team_Task_Manager_Spring/README.md).

| Module | Endpoints |
|---|---|
| **Auth** | `POST /auth/register`, `/auth/login`, `/auth/refresh`, `/auth/logout` |
| **Users** | `GET /users/me`, `PUT /users/me`, `GET /users/` *(admin)*, `PUT /users/{id}/role` *(admin)* |
| **Projects** | `POST /projects/` *(admin)*, `GET /projects/`, `PUT /projects/{id}` *(admin)* |
| **Members** | `POST /projects/{id}/members` *(admin)*, `DELETE /projects/{id}/members/{userId}` *(admin)* |
| **Tasks** | `POST`, `GET`, `PUT`, `PATCH /status`, `DELETE` under `/projects/{projectId}/tasks/` |
| **Comments** | `POST /tasks/{id}/comments`, `DELETE /tasks/{id}/comments/{commentId}` |
| **Join Requests** | `POST`, `GET`, `PATCH` under `/projects/{projectId}/join-requests/` |
| **Dashboard** | `GET /dashboard/my`, `GET /dashboard/admin` *(admin)*, `GET /dashboard/projects/{id}/stats` |

---

## Environment Variables

### Backend (`application.yaml`)

| Variable | Description | Default |
|---|---|---|
| `DB_URL` | PostgreSQL JDBC connection URL | `jdbc:postgresql://localhost:5432/team_task_manager` |
| `DB_USERNAME` | Database username | — |
| `DB_PASSWORD` | Database password | — |
| `JWT_SECRET` | Secret key for signing JWTs (min. 32 chars) | — |
| `SPRING_PROFILES_ACTIVE` | Active Spring profile | `dev` |

### Frontend (`.env`)

| Variable | Description |
|---|---|
| `VITE_API_BASE_URL` | Base URL of the backend API (e.g., `http://localhost:8080/api`) |

---

> Access the live demo at [team-task-manager-react-js.onrender.com](https://team-task-manager-react-js.onrender.com/dashboard) using the admin credentials above.

| View | Description |
|---|---|
| **Dashboard** | Personal stats, recent tasks, overdue items, and project activity |
| **Projects** | Project cards with status, member count, and quick actions |
| **Tasks / Kanban** | Filter tasks by status, priority, and assignee; update via Kanban columns |
| **Team** | View all team members, their roles, and manage access |
| **Admin Panel** | System-wide stats, user management, and full project control |

---

## Author

**Ayush Yadav** — [@Ayushyadav14](https://github.com/Ayushyadav14)
