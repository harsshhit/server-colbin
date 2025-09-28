# Recruitment Platform Backend API

A Node.js + Express + MongoDB backend for a recruitment platform prototype.

## Setup Instructions

1. **Install dependencies:**
   ```bash
   npm install
   ```

2. **Environment variables:**
   - Copy `.env.example` to `.env`
   - Update the values in `.env`:
     ```
     PORT=5000
     MONGODB_URI=mongodb://localhost:27017/recruitment_platform
     JWT_SECRET=your_jwt_secret_key_here
     ```

3. **Start MongoDB:**
   - Ensure MongoDB is running on your system
   - Default connection: `mongodb://localhost:27017`

4. **Run the server:**
   ```bash
   # Development mode with nodemon
   npm run dev
   
   # Production mode
   npm start
   ```

## API Endpoints

### Authentication

#### POST `/api/auth/register`
Register a new user.

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "password123",
  "name": "John Doe"
}
```

**Response:**
```json
{
  "success": true,
  "message": "User registered successfully"
}
```

#### POST `/api/auth/login`
Login with email and password.

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Login successful",
  "token": "jwt_token_here",
  "user": {
    "id": "user_id",
    "email": "user@example.com",
    "name": "John Doe"
  }
}
```

### User Profile

#### GET `/api/user/profile`
Get user profile (Protected route).

**Headers:**
```
Authorization: Bearer jwt_token_here
```

**Response:**
```json
{
  "success": true,
  "user": {
    "id": "user_id",
    "email": "user@example.com",
    "name": "John Doe"
  }
}
```

### Health Check

#### GET `/api/health`
Check server status.

**Response:**
```json
{
  "success": true,
  "message": "Server is running",
  "timestamp": "2024-01-01T00:00:00.000Z"
}
```

## Authentication Flow

1. **Register:** POST to `/api/auth/register` with email, password, and name
2. **Login:** POST to `/api/auth/login` with email and password
3. **Access Protected Routes:** Include JWT token in Authorization header: `Bearer <token>`
4. **Token Expiry:** JWT tokens expire after 1 hour

## Error Handling

All endpoints return consistent error responses:

```json
{
  "success": false,
  "message": "Error description"
}
```

**HTTP Status Codes:**
- `400` - Bad Request (validation errors)
- `401` - Unauthorized (invalid credentials, missing/invalid token)
- `404` - Not Found (invalid routes)
- `500` - Internal Server Error

## Database Schema

### User Model
- `email`: String (unique, required, lowercase)
- `password`: String (required, min 6 characters, hashed with bcrypt)
- `name`: String (required)
- `createdAt`: Date
- `updatedAt`: Date

## Project Structure

```
server/
├── config/
│   └── db.js          # MongoDB connection
├── middleware/
│   └── auth.js        # JWT authentication middleware
├── models/
│   └── User.js        # User schema
├── routes/
│   ├── auth.js        # Authentication routes
│   └── user.js        # User profile routes
├── .env.example       # Environment variables template
├── index.js           # Main application entry point
├── package.json       # Dependencies and scripts
└── README.md          # This file
```
