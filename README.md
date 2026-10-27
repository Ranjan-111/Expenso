# Expenso

## Problem Statement

Managing personal finances across multiple income sources, expense categories, and receipts is often fragmented. Users typically rely on scattered notes, spreadsheets, or memory, which makes it difficult to track spending patterns, stay within budget, and keep receipts organized for tax or reimbursement purposes. There is a need for a single, unified application where users can log income and expenses, categorize them, visualize trends, and digitize receipts without manual data entry.

## Target Users

* Individuals and freelancers who want to track personal income and expenses
* Users who want to digitize and extract data from paper receipts
* Anyone who wants visual analytics (charts) of their spending by category over time

## Solution

**Expenso** is a full-stack personal finance management application that lets users:
* Register and authenticate securely with JWT-based login/signup
* Add, view, update, and delete income and expense transactions with categories
* View analytics and charts for income vs. expense trends and category breakdowns
* Upload receipts and automatically extract expense details using AI-powered OCR (Google Gemini)
* Manage their profile and delete their account permanently

## Architecture

Expenso follows a decoupled client-server architecture:

* **Frontend (React + Vite):** Single-page application with React Router for navigation, TailwindCSS for styling, Axios for API calls, and context-based state management (auth, theme, currency).
* **Backend (Node.js + Express):** REST API server handling authentication (JWT), transaction CRUD, receipt uploads (Multer), analytics aggregation, and account management.
* **Database (MongoDB + Mongoose):** Stores users, transactions, categories, budgets, and receipt metadata.
* **AI Integration (Google Gemini AI):** OCR-based receipt scanning to extract expense details from uploaded images.
* **File Storage:** Receipts are stored locally under `backend/uploads/` and served statically.

## Features

* **Authentication** – JWT-based login & signup
* **Transactions Management** – Add income and expenses with categories
* **Analytics & Charts** – Visual breakdown by category, income/expense trends
* **Receipt Management** – Upload receipts and automatically extract expense details using **Google Gemini OCR**
* **Account Settings** – View your profile and delete your account permanently from the app.

## Tech Stack

**Frontend:**

* React + Vite
* React Router
* Axios
* TailwindCSS

**Backend:**

* Node.js + Express
* MongoDB + Mongoose
* JWT Authentication
* Multer (for file uploads)
* Google Gemini AI SDK (for OCR)

**Dev Tools:**

* Nodemon
* dotenv

**Hosting:**

* Frontend → Netlify / Vercel
* Backend → Render / Railway
* Database → MongoDB Atlas

### Project Structure

```
.
├── backend/
│   ├── server.js # Express app entry
│   ├── package.json
│   ├── config/
│   │   └── db.js
│   ├── routes/
│   │   ├── authRoutes.js
│   │   ├── transactionRoutes.js
│   │   ├── receiptRoutes.js
│   │   └── userRoutes.js
│   ├── middleware/
│   ├── controllers/
│   ├── models/
│   └── uploads/ # static served files (receipts)
├── docs/
│   └── openapi.yaml
├── frontend/
│   ├── src/
│   │   ├── pages/
│   │   ├── components/
│   │   ├── contexts/
│   │   ├── api/
│   │   ├── config/
│   │   └── hooks/
│   ├── App.jsx
│   ├── main.jsx
│   ├── package.json
│   ├── vite.config.js
│   └── tailwindcss.config.js
```

## Getting Started

### Clone the repository

```bash
git clone https://github.com/your-username/expenso.git
cd expenso
```

### Backend Setup

```bash
cd backend
npm install
```

Create a **`.env`** file in the `backend/` folder:

```env
PORT=5000
MONGO_URI=your-mongodb-atlas-uri
JWT_SECRET=your-secret-key
GEMINI_API_KEY=your-gemini-api-key
KEEP_ALIVE_URL=http://localhost:5000
```

Start the backend:

```bash
npm run dev
```

Backend will run on → `http://localhost:5000`

### Frontend Setup

```bash
cd frontend
npm install
```

Create a **`.env`** file in the `frontend/` folder:

```env
VITE_API_URL=http://localhost:5000/api
```

Start the frontend:

```bash
npm run dev
```

Frontend will run on → `http://localhost:5173`

## API Documentation

The full API reference is defined in **OpenAPI 3.0** format.

See the file → [`docs/openapi.yaml`](./docs/openapi.yaml)

You can:

* Open it in [Swagger Editor](https://editor.swagger.io/)
* Import into **Postman** or **Insomnia**

## Core API Endpoints

### Auth

* `POST /api/auth/signup` → Register new user
* `POST /api/auth/login` → Login user
* `GET /api/auth/me` → Fetch logged-in user profile

### Transactions

* `GET /api/transactions` → Get all transactions (paginated)
* `POST /api/transactions` → Create a new transaction
* `GET /api/transactions/summary` → Get income, expense, balance, and recent transactions
* `GET /api/transactions/charts` → Get data for dashboard charts
* `GET /api/transactions/categories/expense` → Get unique expense transaction categories
* `GET /api/transactions/categories/income` → Get unique income transaction categories
* `DELETE /api/transactions/category` → Delete a custom category

### Analytics

* `GET /api/analytics/summary` → Income vs Expense summary
* `GET /api/analytics/categories` → Expense breakdown by category

### Receipts

* `POST /api/receipts/upload` → Upload receipt, trigger Gemini OCR, and create a transaction in one step

### Users

* `DELETE /api/users/account` → Delete the authenticated user account permanently

## Environment Variables

Required configuration is documented below. All secrets use placeholder values — **never commit real credentials**. Copy `.env.example` to `.env` and fill in your own values locally.

### Backend (`backend/.env`)

| Variable | Description | Example |
|----------|-------------|---------|
| `PORT` | Backend server port | `5000` |
| `MONGO_URI` | MongoDB connection string | `your-mongodb-atlas-uri` |
| `JWT_SECRET` | Secret for signing JWT tokens | `your-secret-key` |
| `GEMINI_API_KEY` | Google Gemini AI API key for OCR | `your-gemini-api-key` |
| `KEEP_ALIVE_URL` | URL to keep backend awake | `http://localhost:5000` |

### Frontend (`frontend/.env`)

| Variable | Description | Example |
|----------|-------------|---------|
| `VITE_API_URL` | Backend API base URL | `http://localhost:5000/api` |

> ⚠️ **Security:** `.env` files are gitignored and must never be committed. Real API keys and secrets should only exist in your local environment and your hosting provider's dashboard.

## Deployment

### Backend

* Configure **Start Command**: `npm start`
* Add environment variables in your hosting dashboard
* Example: `https://your-backend.onrender.com`

### Frontend

* Build Command: `npm run build`
* Publish Directory: `dist`
* Environment Variable: `VITE_API_URL=https://your-backend.onrender.com/api`