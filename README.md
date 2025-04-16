# A Basic Microservice Web App with Node.js, MongoDB, and Docker Compose

## Overview
This project involved the containerization and deployment of a full-stack yolo application using Docker. The application consists of a React frontend, Node.js backend, and a MongoDB database, all running as microservices orchestrated by Docker Compose.

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
