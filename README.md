# PARC Platform

- **Backend:** Django (Python) + **PostgreSQL**
- **Frontend:** React (JavaScript) + Vite (dev server)

This README explains how to set up and run the project **locally in VS Code**.

---

## Prerequisites

Install these first:

- **Git**
- **Python 3.10+** (recommended)
- **Node.js 18+** and **npm**
- **PostgreSQL 14+**
- **VS Code**

Recommended VS Code extensions:

- **Python** (Microsoft)
- **ESLint**
- **Prettier**
- **Django** (optional)
- **PostgreSQL** (optional)

---

## 1) Clone the repository and open in VS Code

```bash
git clone https://github.com/Punith2590/parcplatformfull.git
cd parcplatformfull
code .
```

---

## 2) Database Setup (PostgreSQL)

Open PostgreSQL (via `psql` CLI or pgAdmin) and create a database and user.

Example commands (psql):

```sql
CREATE DATABASE parc_db;
CREATE USER obes_user WITH PASSWORD 'parc@123456';
GRANT ALL PRIVILEGES ON DATABASE parc_db TO parc_user;
```

Keep these values ready for your environment variables:

- DB name  
- DB user  
- DB password  
- DB host (usually `localhost`)  
- DB port (usually `5432`)

---

## 3) Backend (Django) Setup — Terminal 1

Open Terminal 1 in VS Code.

### 3.1 Go to the backend directory

```bash
cd backend
```

### 3.2 Create and activate the virtual environment

**Windows (PowerShell / CMD):**
```bash
python -m venv venv
venv\Scripts\activate
```

**macOS / Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

### 3.3 Install dependencies

```bash
pip install -r requirements.txt
```

### 3.4 Configure Environment Variables (IMPORTANT)

This project uses environment variables to keep sensitive credentials secure. You must create a local `.env` file before running the server.

Copy the provided template file:

**Windows:**
```dos
copy .env.example .env
```

**macOS / Linux:**
```bash
cp .env.example .env
```

Open the newly created `.env` file in VS Code and update it with your local PostgreSQL credentials and a secure Django Secret Key.

### 3.5 Run migrations

Generate the database tables:

```bash
python manage.py makemigrations
python manage.py migrate
```

---

### 3.7 Create a Django admin user (recommended)

```bash
python manage.py createsuperuser
```

### 3.8 Start the backend server

```bash
python manage.py runserver
```

Backend usually runs at:
- http://127.0.0.1:8000/

---

## 4) Frontend (React) setup & run

Open the **second terminal** (keep backend running in the first terminal).

### 4.1 Go to frontend folder

From the repository root:
```bash
cd frontend
```

### 4.2 Install frontend dependencies

```bash
npm install
```

### 4.3 Configure frontend environment variables

Look for:
- `frontend/.env.local.example`

Then create a `.env.local` file in the frontend folder and copy the contents of the `.env.local.example` file 

Common Vite pattern:
```env
VITE_API_URL=http://127.0.0.1:8000
```

### 4.4 Start the frontend dev server

```bash
npm run dev
```

Frontend usually runs at:
- http://localhost:5173/

Open the link shown in the terminal.

### 4.5 Login to the admin portal

You can use the superuser credentials you created in the backend to go to the admin portal

---

## 5) Typical development workflow (VS Code)

- Terminal 1:
  - activate venv
  - `python manage.py runserver`
- Terminal 2:
  - `npm run dev`

---

## Troubleshooting

### Port already in use
Run on a different port.

Backend:
```bash
python manage.py runserver 8001
```

Frontend:
```bash
npm run dev -- --port 5174
```

### Django can’t connect to Postgres
Check:
- Postgres is running
- DB name/user/password/host/port match your `.env` and Django settings
- You created the database

### CORS errors (frontend calling backend)
If you see CORS issues, you likely need to allow the frontend origin in Django.
Common allowed origins:
- http://localhost:5173
- http://127.0.0.1:5173

(Exact fix depends on whether the project uses `django-cors-headers`.)

---

## Production notes (optional)
For production deployments you typically:
- build frontend (`npm run build`)
- configure Django for static files, security settings, and production DB
- use a proper server (Gunicorn/Uvicorn + Nginx) rather than `runserver`

---

## Repository structure (expected)

```text
parcplatformfull/
  backend/   # Django project
  frontend/  # React app
```
