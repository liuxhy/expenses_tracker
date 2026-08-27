# Expenses Tracker

A full-stack web application for tracking personal expenses and managing financial transactions.

## Features

- User authentication and authorization (JWT)
- Create and manage income/expense categories
- Track income and expense transactions, filterable by date range, type and category
- Dashboard with an income-vs-expense doughnut chart
- Responsive design with modern UI components

## Tech Stack

### Backend
- **Node.js** with **Express 5** - Server framework
- **MongoDB** with **Mongoose 8** - Database and ODM
- **jsonwebtoken** - Authentication
- **bcryptjs** - Password hashing
- **express-async-handler** - Async error propagation
- **CORS** - Cross-origin resource sharing

### Frontend
- **React 19** - UI library
- **Vite 7** - Build tool and dev server
- **Redux Toolkit** - Auth state management
- **TanStack Query** - Server state management
- **Tailwind CSS v4** - Styling
- **Chart.js** with **react-chartjs-2** - Data visualization
- **Formik** + **Yup** - Form handling and validation
- **Axios** - HTTP client
- **HeadlessUI** + **Heroicons** + **react-icons** - UI components

## Project Structure

```
expenses_tracker/
├── backend/
│   ├── controllers/          # Request handlers
│   │   ├── usersCtrl.js
│   │   ├── categoryCtrl.js
│   │   └── transactionCtrl.js
│   ├── middlewares/          # isAuth (JWT), error handler
│   ├── model/                # Mongoose schemas (User, Category, Transaction)
│   ├── routes/               # API routes
│   │   ├── userRouter.js
│   │   ├── categoryRouter.js
│   │   └── transactionRouter.js
│   ├── .env                  # Environment variables (not in git)
│   ├── .env.example          # Environment variables template
│   ├── app.js                # Express app setup + entry point
│   └── package.json          # Backend dependencies
│
├── frontend/
│   ├── src/
│   │   ├── components/       # Feature components (Users, Category, Transactions, Navbar, Alert, Auth, Home)
│   │   ├── redux/            # Store and auth slice
│   │   ├── services/         # Axios API clients
│   │   ├── utils/            # BASE_URL, token helper
│   │   ├── App.jsx           # Routes
│   │   └── main.jsx          # Entry point (Redux + React Query providers)
│   ├── public/               # Static assets
│   ├── package.json          # Frontend dependencies
│   └── vite.config.js        # Vite configuration
│
├── .gitignore                # Git ignore rules
└── README.md                 # Project documentation
```

## Prerequisites

- **Node.js** 20.19+ or 22.12+ (required by Vite 7; the backend alone runs on 18+)
- **MongoDB** account (MongoDB Atlas or a local installation)
- **npm**

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd expenses_tracker
```

2. Install backend dependencies:
```bash
cd backend
npm install
```

3. Install frontend dependencies:
```bash
cd ../frontend
npm install
```

## Configuration

### Backend Setup

1. Copy the example environment file:
```bash
cd backend
cp .env.example .env
```

2. Edit `.env` and set the following variables:

```env
PORT=8000
MONGODB_URI=your_mongodb_connection_string_here
JWT_SECRET=your_secure_jwt_secret_key_here
FRONTEND_URL=http://localhost:5173
```

**Important:**
- `MONGODB_URI` — your actual MongoDB connection string.
- `JWT_SECRET` — used to sign and verify auth tokens. Generate a strong random value:
  ```bash
  node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
  ```
  Changing this value invalidates all previously issued tokens, so every user has to log in again.
- `FRONTEND_URL` — the origin allowed by CORS. Update it if the frontend runs on a different port.

### Frontend Setup

No environment file is needed. The API base URL is currently hard-coded in
`frontend/src/utils/url.js`:

```js
export const BASE_URL = "http://localhost:8000/api/v1";
```

If the backend runs somewhere else, edit that file. (Moving this to a
`VITE_API_URL` environment variable is a worthwhile improvement, but it is
not implemented yet.)

## Running the Application

### Development Mode

1. Start the backend server:
```bash
cd backend
node app.js
```
The backend runs on `http://localhost:8000`. (There is no `npm start` script defined.)

2. In a new terminal, start the frontend dev server:
```bash
cd frontend
npm run dev
```
The frontend runs on `http://localhost:5173`.

### Production Build

1. Build the frontend:
```bash
cd frontend
npm run build
```

2. Preview the production build:
```bash
npm run preview
```

## API Endpoints

All routes are prefixed with `/api/v1`. Every route except register and login
requires an `Authorization: Bearer <token>` header.

### User Routes
| Method | Path | Auth | Description |
|---|---|---|---|
| POST | `/api/v1/users/register` | No | Register a new user |
| POST | `/api/v1/users/login` | No | Log in, returns a JWT |
| GET | `/api/v1/users/profile` | Yes | Get the current user's profile |
| PUT | `/api/v1/users/change-password` | Yes | Change password |
| PUT | `/api/v1/users/update-profile` | Yes | Update username and email |

### Category Routes
| Method | Path | Auth | Description |
|---|---|---|---|
| POST | `/api/v1/categories/create` | Yes | Create a category |
| GET | `/api/v1/categories/lists` | Yes | List the current user's categories |
| PUT | `/api/v1/categories/update/:categoryId` | Yes | Update a category |
| DELETE | `/api/v1/categories/delete/:id` | Yes | Delete a category |

### Transaction Routes
| Method | Path | Auth | Description |
|---|---|---|---|
| POST | `/api/v1/transactions/create` | Yes | Create a transaction |
| GET | `/api/v1/transactions/lists` | Yes | List transactions (see query params below) |
| PUT | `/api/v1/transactions/update/:id` | Yes | Update a transaction |
| DELETE | `/api/v1/transactions/delete/:id` | Yes | Delete a transaction |

`GET /api/v1/transactions/lists` accepts optional query parameters:

- `startDate`, `endDate` — ISO date strings, filter on the transaction date
- `type` — `income` or `expense`
- `category` — a category name, or `All` to skip category filtering

Results are sorted by transaction date, newest first.

## Data Model

- **User** — `username`, `email` (both unique), hashed `password`
- **Category** — `user`, `name`, `type` (`income` | `expense`)
- **Transaction** — `user`, `type` (`income` | `expense`), `category` (stored as a
  name string, not a reference), `amount`, `date`, `description`

All queries are scoped to the authenticated user, so users only ever see their
own categories and transactions.

## Security Notes

- **Never commit the `.env` file.** It contains the database credentials and the JWT secret.
- `.env.example` is safe to commit — it holds placeholders only.
- Use a strong, randomly generated `JWT_SECRET` (at least 32 bytes).
- In production, set `FRONTEND_URL` to your real frontend origin so CORS stays restricted.
- Tokens are issued with a 30-day expiry and stored in `localStorage`. For a
  production deployment, consider shorter-lived access tokens plus a refresh
  token in an httpOnly cookie.

## License

ISC
