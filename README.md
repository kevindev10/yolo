# A Basic Microservice Web App with Node.js, MongoDB, and Docker Compose

## Overview
This project involved the containerization and deployment of a full-stack yolo application using Docker. The application consists of a React frontend, Node.js backend, and a MongoDB database, all running as microservices orchestrated by Docker Compose.

---



## Core Project Structure

The following is the folder structure for this project:


```plaintext
project-root/
├── client/                    # Frontend application
│   ├── src/                   # Source code (React components, styles, etc.)
│   │   ├── components/
│   │   ├── pages/
│   │   ├── images/            # Images, icons, or other static assets
│   │   └── App.js
│   ├── public/                # Static files (HTML template, favicon, etc.)
│   ├── package.json           # Frontend dependencies
│   └── Dockerfile             # Dockerfile for client container
├── backend/                   # Backend application
│   ├── routes/                # Express routes for handling APIs
│   ├── models/                # MongoDB schemas and models
│   ├── server.js              # Backend entry point
│   ├── package.json           # Backend dependencies
│   └── Dockerfile             # Dockerfile for backend container
├── db/                        # MongoDB initialization scripts (optional)
│   └── init-db.js
├── docker-compose.yml          # Orchestration of all services
├── explanation.md              # Project explanation (documentation)
├── README.md                   # Project overview and setup instructions
└── .gitignore                  # Files to exclude from Git tracking
```

---



## Key Features
- **Frontend:** A React-based interface for adding and managing products.
- **Backend:** A Node.js API for handling requests and business logic.
- **Database:** MongoDB for persistent data storage.
- **Docker Compose:** Simplifies container orchestration for the microservices.

---

## Requirements
Ensure the following are installed on your system:
- [Node.js](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-18-04)  
- npm  
- [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)  
  (Start the MongoDB service with `sudo service mongod start`.)

---

## Setup Instructions

### Frontend Setup
1. Navigate to the `client` folder:
   ```bash
   cd client
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the app:
   ```bash
   npm start
   ```

### Backend Setup
1. Open a new terminal and navigate to the `backend` folder:
   ```bash
   cd ../backend
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the backend server:
   ```bash
   npm start
   ```

---

## Usage
- Visit the app in your browser.
- Add a product using the provided form.  
  *(Note: The price field only accepts numeric input.)*

---

## Highlights
- Persistent data storage with Docker volumes.
- Custom networking for seamless communication between microservices.
- Images tagged and uploaded to DockerHub for streamlined deployment.
- Step-by-step setup for easy replication in any environment.

---
