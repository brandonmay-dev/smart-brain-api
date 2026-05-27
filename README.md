# Smart Brain API

Smart Brain API is the Node.js and Express backend for the Smart Brain full-stack AI face detection application. It handles user registration, sign-in, secure password hashing, PostgreSQL persistence, user image-submission tracking, and server-side Clarifai face-detection requests.

This backend keeps sensitive API credentials off the frontend and provides the REST API layer that connects the React client, PostgreSQL database, and Clarifai AI service.

- 👉 **Frontend Repo:** [smart-brain](https://github.com/brandonmay-dev/smart-brain)
- 👉 **Backend Repo:** [smart-brain-api](https://github.com/brandonmay-dev/smart-brain-api)
- 👉 **Live Demo:** [smartbrain.brandonmay.dev](https://smartbrain.brandonmay.dev/)
- 👉 **Live API:** [safe-dawn-54877-2bdeb01ab080.herokuapp.com](https://safe-dawn-54877-2bdeb01ab080.herokuapp.com/)

---

## Why This Project Stands Out

This backend demonstrates practical server-side development beyond basic CRUD operations.

It supports a deployed React frontend, manages user data securely, communicates with a PostgreSQL database, protects third-party API credentials on the server, and acts as the bridge between the client and Clarifai’s AI face-detection model.

This project shows backend work involving authentication, database operations, external API integration, CORS configuration, environment variables, and deployment-focused setup.

---

## Key Highlights

- REST API built with Node.js and Express
- User registration and sign-in flow
- Password hashing with `bcryptjs`
- PostgreSQL persistence with Knex
- Server-side Clarifai API integration
- User-specific image submission tracking
- Environment-based configuration for local and deployed environments
- CORS handling for frontend/backend communication
- Separate frontend and backend deployment architecture

---

## What The API Does

- Registers new users with hashed passwords
- Authenticates returning users
- Stores and retrieves user profile data
- Sends submitted image URLs to Clarifai’s face-detection model
- Returns AI detection data to the frontend
- Increments each user’s image-processing count
- Provides health-check endpoints for backend verification

---

## Tech Stack

- **Node.js** – JavaScript runtime for backend development
- **Express** – REST API routing and request handling
- **PostgreSQL** – relational database for user and login data
- **Knex** – SQL query builder for database operations
- **bcryptjs** – password hashing and verification
- **dotenv** – environment variable management
- **cors** – frontend/backend request handling

---

## Architecture Overview

1. React frontend sends authentication or image-processing requests to this API
2. Express receives the request and routes it to the appropriate handler
3. Knex reads from or writes to PostgreSQL
4. Passwords are hashed and verified with `bcryptjs`
5. Image URLs are sent from the backend to Clarifai using a private API key
6. API returns user data, Clarifai response data, or updated entry counts to the frontend

---

## API Endpoints

### `GET /`

Health check endpoint for confirming the server is running.

Example response:

```json
{
  "ok": true
}
```

---

### `GET /health/whoami`

Returns information about the connected PostgreSQL database and current database user.

Useful for verifying database connectivity in deployed environments.

---

### `POST /register`

Creates a new user and stores a hashed password.

Example request:

```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "password": "supersecret"
}
```

---

### `POST /signin`

Authenticates an existing user by comparing the submitted password against the stored password hash.

Example request:

```json
{
  "email": "jane@example.com",
  "password": "supersecret"
}
```

---

### `POST /imageurl`

Accepts a public image URL and forwards it to Clarifai’s face-detection model.

The Clarifai API key is stored on the backend so it is never exposed in the frontend.

Example request:

```json
{
  "input": "https://example.com/face.jpg"
}
```

---

### `PUT /image`

Increments the signed-in user’s image submission count.

Example request:

```json
{
  "id": 1
}
```

---

### `GET /profile/:id`

Returns the stored profile data for a specific user.

---

## Environment Variables

Create a `.env` file in the project root.

### Local PostgreSQL Setup

```env
CLARIFAI_PAT=your_clarifai_pat
DB_HOST=127.0.0.1
DB_PORT=5432
DB_NAME=smart-brain
DB_USER=your_postgres_user
DB_PASSWORD=your_postgres_password
PORT=3001
```

### Deployment Setup

```env
CLARIFAI_PAT=your_clarifai_pat
DATABASE_URL=your_database_url
PORT=3001
```

---

## Database Setup

Make sure PostgreSQL is running, then create the database:

```sql
CREATE DATABASE smart-brain;
```

This API expects `login` and `users` tables.

### `login` Table

```sql
CREATE TABLE login (
  id serial PRIMARY KEY,
  hash varchar(100) NOT NULL,
  email text UNIQUE NOT NULL
);
```

### `users` Table

```sql
CREATE TABLE users (
  id serial PRIMARY KEY,
  name varchar(100),
  email text UNIQUE NOT NULL,
  hash varchar(100) NOT NULL,
  entries bigint DEFAULT 0,
  joined timestamp NOT NULL
);
```

---

## Local Development

Install dependencies:

```bash
npm install
```

Run in development mode:

```bash
npm run dev
```

Run in production mode:

```bash
npm start
```

The API runs locally at:

```txt
http://localhost:3001
```

---

## Deployment Notes

- `DATABASE_URL` is used automatically when present
- SSL is enabled for hosted PostgreSQL connections
- Clarifai credentials are kept on the server, not the frontend
- CORS is configured to control which frontend origins can access the API
- The deployed frontend domain is `https://smartbrain.brandonmay.dev`
- If the frontend URL changes, update the backend CORS allowlist in `server.js`

---

## What This Project Demonstrates

- Building a REST API for a separate frontend client
- Implementing authentication with hashed passwords
- Working with relational data using PostgreSQL and Knex
- Protecting third-party API secrets on the server
- Integrating external AI services into backend workflows
- Managing local and production environment configuration
- Supporting deployment-specific database behavior
- Separating frontend and backend responsibilities in a full-stack app

---

## Future Improvements

- Add database migrations and seed files
- Add validation middleware for request bodies
- Add automated route and integration tests
- Move CORS allowlist values into environment variables
- Split `server.js` into routes, controllers, and services
- Add centralized error handling
- Add logging and rate limiting
- Add token-based authentication with JWT or sessions

---

## Author

Brandon May

---

## License

MIT
