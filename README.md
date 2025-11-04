# ChatMe - Full-Stack Real-Time Chat Application

ChatMe is a full-stack, real-time chat application built with the **MERN stack (MongoDB, Express, React, Node.js)** and **Socket.IO**. It is written entirely in **TypeScript**, offering a robust, type-safe codebase.

The application provides a complete chat experience, including secure user authentication, one-on-one (P2P) messaging, group chat creation and management, live typing indicators, and message reactions.

## Core Features

### 1. Real-Time Chat Functionality (Socket.IO)
* **Live Messaging:** Messages are sent and received in real-time without needing to refresh the page, using **Socket.IO** to push data between clients and the server.
* **Typing Indicators:** Users can see when another user in the chat is actively typing a message, with socket events for "start typing" and "stop typing".
* **Message Reactions:** Users can add and remove emoji reactions to messages, with changes broadcast to all users in the chat in real-time.

### 2. Chat & Group Management
* **One-on-One (P2P) Chats:** Users can search for other registered users by name or email and start a new personal chat.
* **Group Chats:** Users can create new group chats, name them, and add multiple users.
* **Group Management:** Group admins can rename the group, add new members, or remove existing members from the chat.
* **Chat List:** The homepage displays a list of all the user's current chats, showing the latest message for each.

### 3. Full Authentication & User Management
* **Secure Sign-up & Login:** Full user authentication system with `name`, `email`, and `password` fields. Passwords are securely hashed on the server using **bcryptjs**.
* **JSON Web Token (JWT) Authentication:** User sessions are managed using JWTs, which are generated on login and sent with every subsequent API request for secure route protection.
* **Password Reset Flow:** A complete "forgot password" and "reset password" flow.
    * Users can request a password reset, which sends a secure, tokenized link to their email using **Nodemailer**.
    * Users can set a new password via the secure link.
* **User Search:** A protected endpoint allows logged-in users to search for other users to start a chat with.

---

## 🛠️ Technical Showcase & Architecture

This project is a robust demonstration of a modern, full-stack application architecture.

### Backend (Node.js, Express, MongoDB, Socket.IO)
* **Tech Stack:** **MERN (MongoDB, Express, React, Node)** fully written in **TypeScript**.
* **Real-time Layer:** A **Socket.IO** server is integrated with the Express server. It handles real-time events for new messages, typing indicators, and message reactions. Sockets are authenticated and users join "rooms" corresponding to their `chatId` to receive relevant events.
* **MVC Architecture:** The backend follows a clear Model-View-Controller pattern, with logic separated into:
    * **Models (`/models`):** Mongoose schemas for `User`, `Chat`, and `Message`.
    * **Routes (`/routes`):** Express routers for different API resources (user, chat, message).
    * **Controllers (`/controllers`):** Contain all the business logic for handling requests (e.g., `createGroupChat`, `sendMessage`).
* **Database Modeling:** Mongoose models use `ref` properties to create relationships between documents (e.g., a `Message` references its `sender` (User) and `chat` (Chat)), allowing for data population.
* **Security:**
    * **Authentication:** `jsonwebtoken` is used in a custom `protect` middleware to secure all sensitive API routes.
    * **Password Hashing:** `bcryptjs` is used to hash all user passwords before saving them to the database.
    * **Production Middleware:** Includes `express-rate-limit` to prevent brute-force attacks and custom error handling middleware (`errorHandler.ts`) for clean error responses.

### Frontend (React, Vite, TailwindCSS)
* **Tech Stack:** **React 18** (with Hooks) built with **Vite** and written in **TypeScript**.
* **State Management:** **React Context API** (`AuthProvider`) is used to manage global authentication state, storing the user and JWT token. This state is persisted in `localStorage` to keep the user logged in.
* **Routing:** **React Router v6** (`react-router-dom`) is used for all client-side routing, including protected routes that check the auth context before rendering a page.
* **API & Socket Connection:**
    * A centralized **Axios** instance (`config/api.ts`) is configured to automatically attach the user's JWT token to the `Authorization` header of every API request.
    * A dedicated `createSocket.ts` utility configures the `socket.io-client` connection, which is then used in the main chat component to emit and listen for events.
* **UI:**
    * **TailwindCSS:** The entire UI is built with TailwindCSS for a modern, responsive, utility-first design.
    * **Component-Based:** The UI is broken into logical components (e.g., `ChatList`, `MessageBubble`, `GroupSettingsModal`), with the main `Homepage` acting as a 2-column layout.
    * **Form Handling:** Uses `react-hook-form` and `zod` for robust, schema-based validation on all forms (login, register, etc.).

---

## 🚀 Technology Stack

* **Frontend:** React 18, TypeScript, Vite, React Router, Axios, Socket.IO Client, TailwindCSS, React Hook Form, Zod
* **Backend:** Node.js, Express.js, TypeScript, MongoDB (with Mongoose), Socket.IO, JSON Web Tokens (JWT), bcryptjs, Nodemailer, Express Rate Limit

---

## 📂 Project Structure
```
/
├── client/ # React (Vite) Frontend
│ ├── src/
│ │ ├── components/ # Reusable UI components (Loader, Inputs)
│ │ ├── config/ # Axios (api.ts) & Socket.IO (createSocket.ts) setup
│ │ ├── contexts/ # React Context (AuthProvider)
│ │ ├── hooks/ # Custom hooks (useAuth)
│ │ ├── pages/ # Application pages (Login, Register, Homepage)
│ │ ├── routes/ # AppRouter.tsx (Protected routes)
│ │ └── ...
│ ├── package.json
│ └── vite.config.ts
│ └── server/ # Node.js (Express) Backend
├── config/ # DB connection, JWT generation
├── controllers/ # API business logic (chat, message, user, reaction)
├── middlewares/ # Auth, error handling, rate limiting
├── models/ # Mongoose schemas (Chat, Message, User)
├── routes/ # Express API route definitions
├── server.ts # Server entry point (Express + Socket.IO setup)
└── package.json
```

---

## API Endpoints Overview

### Auth Routes (`/api/user`)
* `POST /register`: Register a new user.
* `POST /login`: Log in and receive a JWT.
* `POST /forgot-password`: Send a password reset email.
* `PUT /reset-password/:token`: Update password with a valid token.
* `GET /`: Search for users (protected).

### Chat Routes (`/api/chat`)
* `POST /`: Create or access a 1-on-1 chat (protected).
* `GET /`: Fetch all chats for the logged-in user (protected).
* `POST /group`: Create a new group chat (protected).
* `PUT /group/rename`: Rename a group (admin only, protected).
* `PUT /group/add`: Add a user to a group (admin only, protected).
* `PUT /group/remove`: Remove a user from a group (admin only, protected).

### Message Routes (`/api/message`)
* `POST /`: Send a new message to a chat (protected).
* `GET /:chatId`: Get all messages for a specific chat (protected).
* `POST /react`: Add/remove a reaction to a message (protected).

---

## Getting Started

### Prerequisites

* Node.js (v18+)
* npm / yarn / pnpm
* MongoDB (a local instance or a connection URI from MongoDB Atlas)
* A "dummy" Gmail account for `nodemailer` to send password reset emails (or other SMTP provider).

### Installation

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/your-username/chat-me.git](https://github.com/your-username/chat-me.git)
    cd chat-me
    ```

2.  **Install Server Dependencies:**
    ```bash
    cd server
    npm install
    ```

3.  **Install Client Dependencies:**
    ```bash
    cd ../client
    npm install
    ```

### Environment Setup

Create a `.env` file in the `/server` directory and add the following:
```
Server port

PORT=5000

Your MongoDB connection string

MONGO_URI=mongodb+srv://<user>:<password>@cluster.mongodb.net/chat-me?retryWrites=true&w=majority

A strong secret for signing JWTs

JWT_SECRET=your_super_secret_key_123

Environment (development or production)

NODE_ENV=development

Email credentials for password reset

EMAIL_USER=your-email@gmail.com EMAIL_PASS=your-gmail-app-password
```

### Running the Application

1.  **Start the Server:**
    ```bash
    # From the /server directory
    npm run dev
    ```
    *(The server will run on `http://localhost:5000`)*

2.  **Start the Client:**
    ```bash
    # From the /client directory
    npm run dev
    ```
    *(The client will run on `http://localhost:5173`)*
