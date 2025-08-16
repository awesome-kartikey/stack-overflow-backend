# Frequently Asked Questions (FAQ)

**Q1: What is this project?**

A: This project is the backend API service for a web application designed to mimic the core functionalities of Stack Overflow. It handles user authentication, questions, answers, voting, and user profiles.

**Q2: How do I set up the project locally?**

A: Follow these steps:
1.  Clone the repository.
2.  Install dependencies using `npm install`.
3.  Create a `.env` file in the root directory.
4.  Add your MongoDB connection string (`CONNECTION_URL`) and a JWT secret (`JWT_SECRET`) to the `.env` file. You can optionally add a `PORT`.
5.  Run the server using `npm run start`.
(See the [README.md](README.md#setup-instructions) for detailed commands).

**Q3: What database is used?**

A: This project uses MongoDB as its database, interacted with via the Mongoose Object Data Modeling (ODM) library.

**Q4: How is user authentication handled?**

A: Authentication is implemented using JSON Web Tokens (JWT).
*   When a user signs up or logs in successfully, the server generates a JWT containing user identification (`email`, `id`).
*   This token is sent back to the client.
*   For protected routes, the client must send this token in the `Authorization` header as a `Bearer` token.
*   A middleware (`middlewares/auth.js`) verifies the token on incoming requests to protected endpoints.
*   Passwords are securely stored by hashing them using `bcryptjs` before saving them to the database.

**Q5: What are the main API endpoints?**

A: Key endpoints include:
*   User Auth: `POST /user/signup`, `POST /user/login`
*   User Profiles: `GET /user/getAllUsers`, `PATCH /user/update/:id`
*   Questions: `POST /questions/Ask`, `GET /questions/get`, `DELETE /questions/delete/:id`, `PATCH /questions/vote/:id`
*   Answers: `PATCH /answer/post/:id`, `PATCH /answer/delete/:id`
Refer to the `routes/` directory for a complete list.

**Q6: What environment variables are required?**

A: You need to define the following in a `.env` file:
*   `CONNECTION_URL`: Your MongoDB connection string.
*   `JWT_SECRET`: A secret key for signing and verifying JWTs.
*   `PORT` (Optional): The port the server should run on (defaults to 5000).

**Q7: How can I test the API endpoints?**

A: You can use API client tools like:
*   Postman
*   Insomnia
*   `curl` command-line tool
Remember to set the `Content-Type` header to `application/json` for requests with a body and include the `Authorization: Bearer <token>` header for protected routes.

**Q8: Is there a frontend application available?**

A: This repository contains *only* the backend API. A separate frontend application would be needed to interact with this API visually in a browser.

**Q9: What does the `Procfile` do?**

A: The `Procfile` (specifically `web: npm run start`) is typically used by Platform-as-a-Service (PaaS) providers like Heroku to know how to start the web process for your application upon deployment.

**Q10: How is the code structured?**

A: The project follows a common Node.js structure:
*   `index.js`: Entry point, server setup, middleware configuration, DB connection.
*   `routes/`: Defines API endpoints and connects them to controllers.
*   `controllers/`: Contains the business logic for handling requests.
*   `models/`: Defines Mongoose schemas for database collections (Users, Questions).
*   `middlewares/`: Contains custom middleware functions (like authentication).
*   `.env`: Stores confidential environment variables.
*   `package.json`: Lists dependencies and scripts.

---