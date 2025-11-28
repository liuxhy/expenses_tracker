# Expenses Tracker

A full-stack web application for tracking personal expenses and managing financial transactions.

## Features

- User authentication and authorization (JWT)
- Create and manage expense categories
- Track income and expense transactions
- Visual data representation with charts
- Responsive design with modern UI components

## Tech Stack

### Backend
- **Node.js** with **Express.js** - Server framework
- **MongoDB** with **Mongoose** - Database and ODM
- **JWT** - Authentication
- **bcryptjs** - Password hashing
- **CORS** - Cross-origin resource sharing

### Frontend
- **React 19** - UI library
- **Vite** - Build tool and dev server
- **Redux Toolkit** - State management
- **TanStack Query** - Server state management
- **Tailwind CSS** - Styling
- **Chart.js** with **react-chartjs-2** - Data visualization
- **Formik** + **Yup** - Form handling and validation
- **Axios** - HTTP client
- **HeadlessUI** + **Heroicons** - UI components

## Project Structure

```
expenses_tracker/
├── backend/
│   ├── controllers/      # Request handlers
│   ├── middlewares/      # Custom middleware
│   ├── model/            # Mongoose schemas
│   ├── routes/           # API routes
│   │   ├── userRouter.js
│   │   ├── categoryRouter.js
│   │   └── transactionRouter.js
│   ├── utils/            # Helper functions
│   ├── .env              # Environment variables (not in git)
│   ├── .env.example      # Environment variables template
│   ├── app.js            # Express app setup
│   └── package.json      # Backend dependencies
│
├── frontend/
│   ├── src/              # React source files
│   ├── public/           # Static assets
│   ├── package.json      # Frontend dependencies
│   └── vite.config.js    # Vite configuration
│
├── .gitignore            # Git ignore rules
└── README.md             # Project documentation
```

## Prerequisites

- **Node.js** (v14 or higher)
- **MongoDB** account (or local MongoDB installation)
- **npm** or **yarn**

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

2. Edit `.env` and update the following variables:

```env
PORT=8000
MONGODB_URI=your_mongodb_connection_string_here
JWT_SECRET=your_secure_jwt_secret_key_here
FRONTEND_URL=http://localhost:5173
```

**Important:**
- Replace `MONGODB_URI` with your actual MongoDB connection string
- Generate a strong, random string for `JWT_SECRET` (you can use: `node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"`)
- Update `FRONTEND_URL` if your frontend runs on a different port

### Frontend Setup

1. Create a `.env` file in the `frontend` directory (if needed)
2. Configure API endpoint:

```env
VITE_API_URL=http://localhost:8000
```

## Running the Application

### Development Mode

1. Start the backend server:
```bash
cd backend
node app.js
```
The backend will run on `http://localhost:8000`

2. In a new terminal, start the frontend dev server:
```bash
cd frontend
npm run dev
```
The frontend will run on `http://localhost:5173`

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

### User Routes
- `POST /api/users/register` - Register new user
- `POST /api/users/login` - User login
- `GET /api/users/profile` - Get user profile

### Category Routes
- `POST /api/categories` - Create category
- `GET /api/categories` - Get all categories
- `PUT /api/categories/:id` - Update category
- `DELETE /api/categories/:id` - Delete category

### Transaction Routes
- `POST /api/transactions` - Create transaction
- `GET /api/transactions` - Get all transactions
- `GET /api/transactions/:id` - Get single transaction
- `PUT /api/transactions/:id` - Update transaction
- `DELETE /api/transactions/:id` - Delete transaction

## Security Notes

- **Never commit the `.env` file** to version control. It contains sensitive credentials.
- The `.env.example` file is safe to commit as it contains no actual credentials.
- In production, update CORS settings to restrict allowed origins to your actual frontend domain.
- Use a strong, randomly generated JWT secret (minimum 32 characters).
- Consider using environment-specific `.env` files (`.env.development`, `.env.production`) for different environments.

## License

ISC
