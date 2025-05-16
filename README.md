# **Microservice Web Application with Node.js, MongoDB, and Kubernetes**

## **Project Overview**
This project is a fully containerized **microservice web application** deployed on **Google Kubernetes Engine (GKE)**. It features:  
- A **React-based frontend**  
- A **Node.js Express backend**  
- A **MongoDB database with persistent storage**  

The application is designed for **scalability, reliability, and fault tolerance**, leveraging Kubernetes for orchestration.

---

## **Live Deployment on GKE**
🔗 **Access the live application here:**  
### 👉 [**GKE Live URL**](http://35.184.247.235:3000/)

---

## **Technology Stack**
- 🖥️ **Frontend:** React (client-side UI)  
- 🛠️ **Backend:** Node.js + Express (API services)  
- 📦 **Database:** MongoDB (Persistent Data Store)  
- 🏗️ **Containerization:** Docker (for managing microservices)  
- 🚀 **Orchestration:** Kubernetes (GKE)  
- 🔄 **CI/CD:** GitHub + DockerHub  

---

## **Kubernetes Deployment Strategy**

### 1️⃣ Kubernetes Objects Used
- ✔️ **Frontend & Backend:** Deployed as **Kubernetes Deployments** for **rolling updates, scalability, and high availability**.  
- ✔️ **MongoDB:** Implemented using **StatefulSets** for **persistent storage and stable pod identity**.  
- ✔️ **Networking:** Services configured to ensure **secure internal & external communication**.

### 2️⃣ Exposing Services to Internet Traffic
- ✔️ **Frontend & Backend:** `LoadBalancer` Services expose them externally with **public IP addresses**.  
- ✔️ **MongoDB:** Configured as a **headless service** (`ClusterIP: None`), ensuring stability without exposing the database externally.

### 3️⃣ Persistent Storage Implementation
- ✔️ **MongoDB Data is stored in a Persistent Volume (PVC)**
- ✔️ Ensures that database contents **survive pod restarts and deletions**
- ✔️ Kubernetes dynamically reattaches the storage to new MongoDB pods

---

## **How to Deploy Locally**

### Clone the repository
```bash
git clone https://github.com/kevindev10/yolo
cd https://github.com/kevindev10/yolo
```

### Build and run the containers locally using Docker Compose
```bash
docker-compose up --build
```

---

## **Contributors & Acknowledgments**
- 👨‍💻 **Lead Developer:** Kevin  
- 🎯 **Deployment successfully completed on Google Kubernetes Engine (GKE)**  

---

## **Project Structure**
The folder hierarchy follows **clean architecture principles**, keeping services modular and maintainable:

```plaintext
📂 Project Root  
├── client/                # React frontend  
│   ├── src/               # Frontend source code  
│   ├── public/            # Static assets  
│   ├── Dockerfile         # Frontend Docker configuration  
│   ├── package.json       # Frontend dependencies  
│   ├── .env               # Environment variables  
│   └── README.md          # Frontend documentation  
│  
├── backend/               # Node.js Express backend  
│   ├── src/               # Backend source code  
│   ├── models/            # Database models  
│   ├── routes/            # API route handlers  
│   ├── controllers/       # Business logic  
│   ├── config/            # App configuration files  
│   ├── Dockerfile         # Backend Docker configuration  
│   ├── package.json       # Backend dependencies  
│   ├── .env               # Backend environment variables  
│   └── README.md          # Backend documentation  
│  
├── manifests/             # Kubernetes deployment files  
│   ├── frontend-deployment.yaml   # Frontend deployment configuration  
│   ├── backend-deployment.yaml    # Backend deployment configuration  
│   ├── mongo-statefulset.yaml     # MongoDB StatefulSet configuration  
│   ├── service.yaml               # Kubernetes Services definitions  
│   └── storage.yaml               # Persistent Volume Claim configuration  
│  
├── docker-compose.yaml    # Local development setup  
├── README.md              # Project documentation  
└── explanation.md         # Detailed deployment rationale  
```

---