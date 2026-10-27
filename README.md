# Expenso

## Project Name

Expenso

## Problem Statement

What problem are you solving?

Managing personal finances across multiple income sources, expense categories, and receipts is often fragmented. Users typically rely on scattered notes, spreadsheets, or memory, which makes it difficult to track spending patterns, stay within budget, and keep receipts organized for tax or reimbursement purposes. There is a need for a single, unified application where users can log income and expenses, categorize them, visualize trends, and digitize receipts without manual data entry.

## Target Users

Who is the application built for?

Expenso is built for:

* Individuals and freelancers who want to track personal income and expenses
* Users who want to digitize and extract data from paper receipts
* Anyone who wants visual analytics (charts) of their spending by category over time

## Solution

How does your product solve the problem?

Expenso is a full-stack personal finance management application that lets users:

* Register and authenticate securely with JWT-based login/signup
* Add, view, update, and delete income and expense transactions with categories
* View analytics and charts for income vs. expense trends and category breakdowns
* Upload receipts and automatically extract expense details using AI-powered OCR (Google Gemini)
* Search, filter, and paginate through their transaction history
* Export transactions to CSV
* Manage their profile and delete their account permanently

## Key Features

What can users actually do?

* **Authentication** – Secure JWT-based login and signup with bcrypt-hashed passwords
* **Transactions Management** – Create, read, update, and delete income/expense transactions with categories
* **Search & Filtering** – Search transactions by name, filter by income/expense type and category, and filter by date range
* **Analytics & Charts** – View income vs. expense summaries and category breakdowns via visual charts
* **Receipt Management** – Upload receipt images/PDFs and automatically extract merchant, amount, date, and category using Google Gemini AI OCR
* **Export** – Download transaction history as CSV
* **Account Settings** – View profile and delete account permanently

## Tech Stack

What technologies did you use?

**Frontend:**

* React + Vite
* React Router
* Axios
* TailwindCSS
* Context API (state management: auth, theme, currency)

**Backend:**

* Node.js + Express
* MongoDB + Mongoose (persistent data)
* JWT Authentication
* bcrypt (password hashing)
* Multer (file upload handling)
* Google Gemini AI SDK (AI/LLM integration for receipt OCR)
* node-cron (background keep-alive job)

**Dev Tools:**

* Nodemon
* dotenv

**Hosting / Deployment:**

* Frontend → Netlify / Vercel
* Backend → Render / Railway
* Database → MongoDB Atlas

## Architecture

Explain the major components of your application.

Expenso follows a decoupled client-server architecture:

* **Frontend (React + Vite):** Single-page application with React Router for navigation, TailwindCSS for styling, Axios for API calls, and Context API for state management (auth, theme, currency).
* **Backend (Node.js + Express):** REST API server handling authentication (JWT), transaction CRUD, search/filtering, receipt uploads (Multer), analytics aggregation, CSV export, and account management.
* **Database (MongoDB + Mongoose):** Persistent storage for users, transactions, categories, budgets, and receipt metadata. Indexes are defined for efficient filtering and aggregation queries.
* **AI Integration (Google Gemini AI):** OCR-based receipt scanning to extract merchant, amount, date, and category from uploaded images.
* **File Storage:** Receipts are stored locally under `backend/uploads/` and served statically via `/uploads`.
## Local Setup

Provide clear instructions for running the application.

### Prerequisites

* Node.js (v18 or higher)
* npm
* MongoDB Atlas account (or local MongoDB)
* Google Gemini API key

### Backend Setup

```bash
cd backend
npm install
```

Create a **`.env`** file in the `backend/` folder (see Environment Variables section below):

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
## Environment Variables

Document required configuration without exposing secrets.

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

> ⚠️ **Security:** `.env` files are gitignored and must never be committed to GitHub. Real API keys and secrets should only exist in your local environment and your hosting provider's dashboard. Passwords are hashed with bcrypt before storage — never stored in plain text.

## Deployment

Provide links or instructions for accessing the final application.

### Backend → Render / Railway

* Configure **Start Command**: `npm start`
* Add the environment variables listed above in your hosting dashboard
* Example deployed backend: `https://your-backend.onrender.com`

### Frontend → Netlify / Vercel

* Build Command: `npm run build`
* Publish Directory: `dist`
* Environment Variable: `VITE_API_URL=https://your-backend.onrender.com/api`
---

### Project Structure

```
.
├── backend/
│   ├── server.js          # Express app entry
│   ├── package.json
│   ├── config/
│   │   └── db.js          # MongoDB connection
│   ├── routes/
│   │   ├── authRoutes.js
│   │   ├── transactionRoutes.js
│   │   ├── receiptRoutes.js
│   │   ├── userRoutes.js
│   │   ├── budgetRoutes.js
│   │   └── recurringTransactionRoutes.js
│   ├── middleware/
│   │   ├── authMiddleware.js      # JWT authorization
│   │   ├── sanitizeMiddleware.js  # Input sanitization
│   │   ├── validationMiddleware.js
│   │   └── uploadMiddleware.js    # Multer file handling
│   ├── controllers/
│   │   ├── authController.js      # Authentication logic
│   │   ├── transactionController.js
│   │   ├── receiptController.js   # Gemini OCR integration
│   │   ├── userController.js
│   │   ├── budgetController.js
│   │   └── recurringTransactionController.js
│   ├── models/
│   │   ├── User.js           # bcrypt-hashed passwords
│   │   ├── IncomeExpense.js  # Transactions
│   │   ├── Receipt.js
│   │   ├── Budget.js
│   │   └── RecurringTransactions.js
│   ├── uploads/              # Static served receipt files
│   ├── .env                  # (gitignored)
│   └── .env.example
├── docs/
│   └── openapi.yaml          # OpenAPI 3.0 spec
├── frontend/
│   ├── src/
│   │   ├── pages/
│   │   ├── components/
│   │   ├── contexts/         # Auth, Theme, Currency state
│   │   ├── api/
│   │   ├── config/
│   │   └── hooks/
│   ├── App.jsx
│   ├── main.jsx
│   ├── package.json
│   ├── vite.config.js
│   ├── tailwind.config.js
│   ├── .env                  # (gitignored)
│   └── .env.example
├── .gitignore
├── docker-compose.yml
├── README.md
└── LICENSE
```

Frontend will run on → `http://localhost:5173`