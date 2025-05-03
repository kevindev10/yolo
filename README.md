
---

# A Fully Automated Microservice Web Application with Node.js, MongoDB, and Docker Compose  

## Overview  
This project implements containerized microservices using Docker Compose to orchestrate a React frontend, a Node.js backend, and a MongoDB database. Additionally, automation with Vagrant and Ansible ensures streamlined provisioning, removing manual setup steps and enabling a fully automated deployment process.

---

## Project Structure  

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
│   └── Dockerfile             # Dockerfile for frontend container
├── backend/                   # Backend application
│   ├── routes/                # Express routes for handling APIs
│   ├── models/                # MongoDB schemas and models
│   ├── server.js              # Backend entry point
│   ├── package.json           # Backend dependencies
│   └── Dockerfile             # Dockerfile for backend container
├── db/                        # MongoDB initialization scripts (optional)
│   └── init-db.js
├── docker-compose.yml          # Orchestration of all services
├── ansible/                    # Ansible automation scripts
│   ├── playbook.yml            # Defines provisioning tasks
│   ├── roles/                  # Modular automation roles
│   │   ├── system_config/       # Prepares environment and installs dependencies
│   │   ├── docker_setup/        # Installs and configures Docker
│   │   ├── frontend_setup/      # Deploys the frontend container
│   │   ├── backend_setup/       # Deploys the backend container
│   │   ├── mongo_setup/         # Deploys the MongoDB container
│   │   ├── legacy/              # Contains the deprecated app_deployment role for reference
│   │   │   ├── app_deployment/  # Previous deployment role (archived for historical reference)
├── vagrant/                    # Vagrant environment configuration
│   ├── Vagrantfile              # Defines VM setup and automation triggers
├── explanation.md               # Project explanation (documentation)
├── README.md                    # Project overview and setup instructions
└── .gitignore                    # Files to exclude from Git tracking
```

---

## Key Features  
- Automated deployment using Vagrant and Ansible, reducing manual configuration.  
- Containerized microservices for frontend, backend, and database scalability.  
- Persistent storage with Docker volumes, ensuring data retention across restarts.  
- Custom networking for seamless inter-container communication via Docker Compose.  
- Optimized resource usage by leveraging minimal base images where applicable.  

---

## Requirements  
Ensure the following dependencies are installed:  
- Vagrant for virtual machine provisioning  
- Ansible for automated configuration management  
- Docker and Docker Compose for container orchestration  
- Node.js and npm for frontend and backend package management  

---

## Deployment Instructions  

### Automated Setup  
To deploy the entire stack automatically, run the following command in the project root:  
```bash
vagrant up
```
This initializes the virtual machine, provisions necessary configurations, and deploys the application without manual intervention.

---

### Manual Setup (Alternative)  

#### Frontend  
```bash
cd client
npm install
npm start
```

#### Backend  
```bash
cd backend
npm install
npm start
```

#### Database Initialization  
Ensure MongoDB is running before executing database setup commands:  
```bash
sudo service mongod start
node db/init-db.js
```

---

## Application Usage  
- Access the application through a web browser.  
- Utilize the frontend interface to add and manage products.  
- Verify database persistence across multiple sessions using Docker volumes.  

---

## Testing and Debugging  
To verify container status, execute the following commands:  

Check active containers:  
```bash
docker ps
```

View logs for debugging:  
```bash
docker logs <container_id>
```

Inspect service status within Docker Compose:  
```bash
docker-compose ps
```

---

