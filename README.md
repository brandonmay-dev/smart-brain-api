# FaceRecognitionBrain API

FaceRecognitionBrain API is the Express backend for the FaceRecognitionBrain full-stack project. It handles authentication, password hashing, PostgreSQL database operations, and requests to Clarifai for face detection.

- Frontend repo: [facerecognitionbrain](https://github.com/brandonmay-dev/facerecognitionbrain)
- Backend repo: [facerecognitionbrain-api](https://github.com/brandonmay-dev/facerecognitionbrain-api)
- Live API: [safe-dawn-54877-2bdeb01ab080.herokuapp.com](https://safe-dawn-54877-2bdeb01ab080.herokuapp.com/)

## What This API Does

- Registers new users
- Signs users in with hashed password validation
- Sends image URLs to Clarifai's face-detection model
- Updates each user's image submission count
- Returns profile data for a user

## Tech Stack

- Node.js
- Express
- PostgreSQL
- Knex
- bcryptjs
- dotenv
- cors

## Endpoints

- `GET /`
- `GET /health/whoami`
- `POST /register`
- `POST /signin`
- `POST /imageurl`
- `PUT /image`
- `GET /profile/:id`

## Request Flow

1. The frontend sends auth or image requests to this API.
2. The API validates the payload and queries PostgreSQL when needed.
3. Passwords are hashed with `bcryptjs`.
4. Face-detection requests are forwarded to Clarifai using a private API key stored in environment variables.
5. The API returns user records, face-detection data, or updated entry counts back to the frontend.

## Environment Variables

Create a `.env` file in the project root with either local PostgreSQL settings or a deployment connection string.

### Local PostgreSQL setup

```env
CLARIFAI_PAT=your_clarifai_pat
DB_HOST=127.0.0.1
DB_PORT=5432
DB_NAME=smart-brain
DB_USER=your_postgres_user
DB_PASSWORD=your_postgres_password
PORT=3001
```

### Deployment-style database setup

```env
CLARIFAI_PAT=your_clarifai_pat
DATABASE_URL=your_database_url
PORT=3001
```

## Database Setup

Make sure PostgreSQL is running, then create the database:

```sql
CREATE DATABASE smart-brain;
```

This API currently expects PostgreSQL tables named `users` and `login`.

### `login`

```sql
CREATE TABLE login (
  id serial PRIMARY KEY,
  hash varchar(100) NOT NULL,
  email text UNIQUE NOT NULL
);
```

### `users`

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

## Local Development

Install dependencies:

```bash
npm install
```

Run in development:

```bash
npm run dev
```

Run in production:

```bash
npm start
```

The API runs at `http://localhost:3001`.

## Deployment Notes

When deploying, provide `DATABASE_URL` and `CLARIFAI_PAT` through the host environment. If the frontend domain changes, update the backend CORS allowlist to include the deployed frontend origin.

## What This Project Demonstrates

- Building REST endpoints with Express
- Structuring a backend around a separate frontend client
- Hashing passwords securely instead of storing plain text
- Querying PostgreSQL through Knex
- Protecting third-party API credentials on the server
- Managing CORS and deployment configuration

## Good Next Improvements

- Add migrations and seed files
- Add request validation middleware
- Add automated tests for auth and image routes
- Move the CORS allowlist into environment configuration
- Split route logic into controllers and services for cleaner scaling
- Add rate limiting and request logging

## Author

Brandon May

## License

MIT
