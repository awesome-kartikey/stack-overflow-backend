# Awesome Kartikey Stack Overflow Backend

This repository contains the backend API service for a Stack Overflow clone application, built using the MERN stack components (Node.js, Express, MongoDB). It provides endpoints for user authentication, managing questions, answers, and user profiles.

## Features

- **User Authentication**: Secure signup and login using JWT (JSON Web Tokens) and password hashing (bcryptjs).
- **Question Management**:
  - Ask new questions.
  - View all questions.
  - Delete questions (authenticated users, own questions).
  - Upvote/Downvote questions.
- **Answer Management**:
  - Post answers to specific questions.
  - Delete answers (authenticated users).
- **User Profiles**:
  - View basic user details.
  - Update user profiles (name, about, tags).
- **RESTful API**: Follows REST principles for clear and predictable endpoints.

## Tech Stack

- **Backend**: Node.js, Express.js
- **Database**: MongoDB with Mongoose ODM
- **Authentication**: JSON Web Tokens (JWT), bcryptjs (for password hashing)
- **Middleware**: CORS, Express JSON/URLencoded body parsers
- **Environment Variables**: dotenv
- **Development**: Nodemon (for auto-restarting server)
- **Language**: JavaScript (ES Modules)

## Setup Instructions

1.  **Clone the repository:**

    ```bash
    git clone https://github.com/awesome-kartikey/stack-overflow-backend.git
    cd stack-overflow-backend
    ```

2.  **Install dependencies:**

    ```bash
    npm install
    ```

3.  **Set up Environment Variables:**
    Create a `.env` file in the root directory and add the following variables:

    ```dotenv
    # MongoDB Connection String
    CONNECTION_URL=mongodb+srv://<username>:<password>@<cluster-url>/<database-name>?retryWrites=true&w=majority

    # JWT Secret Key (use a strong, random string)
    JWT_SECRET=your_super_secret_jwt_key

    # Port for the server (optional, defaults to 5000)
    PORT=5000
    ```

    - Replace placeholders in `CONNECTION_URL` with your actual MongoDB Atlas (or other provider) credentials and database name.
    - Replace `your_super_secret_jwt_key` with a secure secret.

4.  **Run the server:**
    - For development (with automatic restarts on file changes):
      ```bash
      npm run start
      ```
    - The server will start, typically on `http://localhost:5000` (or the port specified in your `.env` file). You should see the message `server running on port 5000` in the console upon successful connection to the database and server start.

## Usage

The API provides several endpoints to interact with the application data. You can use tools like Postman, Insomnia, or `curl` to test the endpoints.

**Base URL**: `http://localhost:5000` (or your deployed URL)

**Authentication**:

- Most routes under `/questions`, `/answer`, and `/user/update` require authentication.
- Include the JWT token received after login in the `Authorization` header of your requests as a Bearer token:
  `Authorization: Bearer <your_jwt_token>`

**Key Endpoints**:

- `POST /user/signup`: Register a new user.
- `POST /user/login`: Log in and receive a JWT token.
- `GET /user/getAllUsers`: Get a list of all registered users (basic info).
- `PATCH /user/update/:id`: Update a user's profile (requires auth).
- `POST /questions/Ask`: Post a new question (requires auth).
- `GET /questions/get`: Get all questions.
- `DELETE /questions/delete/:id`: Delete a question (requires auth).
- `PATCH /questions/vote/:id`: Upvote or downvote a question (requires auth).
- `PATCH /answer/post/:id`: Post an answer to a question with the given `:id` (requires auth).
- `PATCH /answer/delete/:id`: Delete an answer from a question (requires auth, needs `answerId` in body).

Refer to the `routes/` directory for detailed endpoint definitions and the `controllers/` directory for the corresponding logic.
