# System Architecture

This document outlines the architecture of the "Awesome Kartikey Stack Overflow Backend" project.

## 1. Overview

This project is a **monolithic backend API** built with Node.js and the Express framework. It serves as the server-side component for a Stack Overflow clone application. Its primary responsibilities include:

- Handling incoming HTTP requests from clients (e.g., a web frontend, mobile app).
- Managing user authentication and authorization.
- Performing CRUD (Create, Read, Update, Delete) operations on data (Users, Questions, Answers).
- Interacting with a MongoDB database via Mongoose.
- Returning responses (typically JSON) to the client.

## 2. Technology Stack

- **Runtime Environment**: Node.js
- **Web Framework**: Express.js
- **Database**: MongoDB
- **ODM**: Mongoose
- **Authentication**: JWT (jsonwebtoken library), bcryptjs (password hashing)
- **Language**: JavaScript (ES Modules)

## 3. Folder Structure

The project is organized into logical directories to promote separation of concerns:

```
stack-overflow-backend/
│
├── index.js         # Main application entry point, server setup, DB connection
├── package.json     # Project metadata, dependencies, scripts
├── .env             # Environment variables (Gitignored)
├── Procfile         # Deployment configuration (e.g., for Heroku)
│
├── controllers/     # Request handling logic (business logic)
│   ├── Answers.js   # Logic for answer-related operations
│   ├── auth.js      # Logic for signup and login
│   ├── Questions.js # Logic for question-related operations
│   └── users.js     # Logic for user profile operations
│
├── middlewares/     # Custom middleware functions
│   └── auth.js      # JWT authentication verification middleware
│
├── models/          # Mongoose schema definitions
│   ├── auth.js      # User schema definition
│   └── Questions.js # Question (and embedded Answer) schema definition
│
└── routes/          # API route definitions
    ├── Answers.js   # Routes related to answers (/answer)
    ├── Questions.js # Routes related to questions (/questions)
    └── users.js     # Routes related to users (/user)
```

## 4. Major Components

- **`index.js` (Entry Point)**:
  - Initializes the Express application (`app`).
  - Connects to the MongoDB database using Mongoose.
  - Loads environment variables using `dotenv`.
  - Configures core middleware (CORS, body parsing).
  - Mounts the route handlers (`userRoutes`, `questionRoutes`, `answerRoutes`).
  - Starts the HTTP server, listening on the configured port.
- **Express Routers (`routes/`)**:
  - Define API endpoints (paths and HTTP methods like GET, POST, PATCH, DELETE).
  - Map incoming requests to specific controller functions.
  - Apply middleware (like the `auth` middleware) to specific routes or routers.
- **Controllers (`controllers/`)**:
  - Contain the core application logic for each route.
  - Receive `req` (request) and `res` (response) objects from Express.
  - Interact with Models (`models/`) to query or update data in the database.
  - Process data and formulate the response to be sent back to the client.
- **Models (`models/`)**:
  - Define the structure (schema) of data stored in MongoDB using Mongoose.
  - Provide an interface for creating, querying, updating, and deleting documents in the database collections.
  - Include data validation rules and default values.
- **Middleware (`middlewares/`)**:
  - Functions that execute during the request-response cycle.
  - Used for cross-cutting concerns like:
    - Authentication (`auth.js`): Verifies JWT tokens.
    - Logging (not implemented here, but could be added).
    - Input validation (partially handled in controllers, could be middleware).
    - Error handling (basic error handling in controllers, could be centralized).

## 5. Data Flow (Example: Asking a Question)

1.  **Client Request**: A logged-in user sends a `POST` request to `/questions/Ask` with question details (title, body, tags) in the request body and the JWT in the `Authorization: Bearer <token>` header.
2.  **Server Entry (`index.js`)**: The Express server receives the request.
3.  **Routing (`routes/Questions.js`)**: The router matches `POST /Ask` and identifies the route handler `AskQuestion` and the required `auth` middleware.
4.  **Authentication Middleware (`middlewares/auth.js`)**:
    - The `auth` middleware executes first.
    - It extracts the token from the `Authorization` header.
    - It verifies the token using `jsonwebtoken` and the `JWT_SECRET`.
    - If valid, it extracts the `userId` from the token payload and attaches it to the `req` object (`req.userId`).
    - It calls `next()` to pass control to the next function (the controller). If invalid, it might stop the request or return an error (current implementation logs the error).
5.  **Controller (`controllers/Questions.js`)**:
    - The `AskQuestion` function executes.
    - It accesses the question data from `req.body` and the authenticated user's ID from `req.userId`.
    - It creates a new `Question` model instance using the data.
    - It calls the `save()` method on the model instance.
6.  **Model/Mongoose (`models/Questions.js`)**:
    - Mongoose translates the `save()` call into a MongoDB `insertOne` command using the defined `QuestionSchema`.
7.  **Database (MongoDB)**: The new question document is inserted into the `questions` collection.
8.  **Response Flow**:
    - MongoDB confirms the successful insertion back to Mongoose/Model.
    - The `save()` method resolves successfully in the controller.
    - The controller sends a success response (status 200 with a JSON message) back to the client via the `res.status().json()` method.

## 6. Design Decisions

- **Monolithic Architecture**: Suitable for the current scale and complexity. Simplifies development and deployment initially.
- **Express.js**: A minimal and flexible Node.js web application framework. Widely adopted, large community support.
- **MongoDB/Mongoose**: MongoDB offers schema flexibility, suitable for evolving requirements. Mongoose provides structure, validation, and simplifies database interactions. Embedding answers within the question document (`Questions` model) is efficient for read operations (fetching a question and its answers together) but might require careful handling for updates/deletions of specific answers.
- **JWT Authentication**: Stateless authentication mechanism, suitable for REST APIs. Avoids the need for server-side sessions.
- **Separation of Concerns**: Dividing the application into `routes`, `controllers`, `models`, and `middlewares` makes the codebase more organized, maintainable, and testable.
- **ES Modules (`type: "module"`)**: Uses the modern standard JavaScript module system (`import`/`export`).
- **Environment Variables (`dotenv`)**: Keeps sensitive information (database credentials, JWT secret) out of the codebase, improving security.

---
