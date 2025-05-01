
---

# **Microservice Web Application with Node.js, MongoDB, and Docker Compose**  

## **Choice of Base Image**  

- **Why `node:14`?**  
  Node.js 14 is a stable and widely supported version, ensuring reliability for most applications. It provides an optimal balance between performance and compatibility.  

- **Why Alpine?**  
  Alpine Linux is a lightweight base image designed for minimal resource consumption. It significantly reduces the final image size, which is beneficial in production environments where large images can slow down deployment and increase storage costs.  

---

## **Dockerfile Directives**  

### **1. `FROM`**  
Defines the base image for the container.  

### **2. `WORKDIR`**  
Sets the working directory inside the container. All subsequent file operations (`COPY`, `RUN`, etc.) occur relative to this directory.  

### **3. `COPY`**  
Transfers files from the host machine into the container.  

### **4. `RUN`**  
Executes commands within the container at **build time**, not runtime. Used for installing dependencies or configuring system settings.  

### **5. `EXPOSE`**  
Documents which port the application listens on. While it does not actually publish the port, it assists in container networking configurations.  

### **6. `CMD`**  
Specifies the default command that runs when the container starts. Unlike `RUN`, it executes **at runtime** rather than during image creation.  

---

## **Docker Compose Networking**  

### **Bridge Network (`app-net`)**  

The application defines a **custom bridge network** to facilitate seamless communication between microservices.  

```plaintext
- Network Name: `app-net` (explicitly named for easy identification).  
- Driver: `bridge` (default network type for Docker, enabling direct container communication).  
- Attachable: `true` (allows external containers to join dynamically).  
- IPAM (IP Address Management):  
  - Subnet: `172.20.0.0/16` (custom IP range for controlled address allocation).  
  - IP Range: `172.20.0.0/16` (ensures predictable IP assignments).  
```

### **Inter-Service Communication**  

Containers within the same network can interact using their **service names as hostnames**, eliminating the need for explicit IP configurations.  

For example:
- The backend (`yolo-backend`) can connect to MongoDB (`app-ip-mongo`) using the hostname `app-mongo`.  
- The frontend (`yolo-client`) interacts with the backend (`yolo-backend`) without requiring hardcoded IPs.  

This architecture enhances **scalability and modular service integration**.  

---

## **Docker Compose Volume Management**  

The Docker Compose configuration includes **data persistence mechanisms**, ensuring storage reliability across container lifecycles.  

### **Workflow Summary**  

```plaintext
1. **Volume Initialization:** When `docker-compose up` is executed, Docker automatically creates the volume (`app-mongo-data`).  
2. **Data Storage:** MongoDB reads/writes data from `/data/db`, backed by `app-mongo-data`.  
3. **Persistence:** The volume ensures MongoDB retains data even if containers are restarted or rebuilt.  
```

This volume setup prevents data loss during updates, container reboots, or environment migrations.  

---

## **IP 3 Configuration Management**  

### **Order of Execution in the Playbook**  

The playbook executes tasks **sequentially**, ensuring dependencies are properly configured before proceeding to the next step.  

#### **Playbook Structure**  
```yaml
- name: Full Automated Setup
  hosts: all
  become: true
  roles:
    - system_config
    - docker_setup
    - app_deployment
```

Each role follows a specific order of execution, ensuring a smooth provisioning process.  

---

### **Role Breakdown and Justification**  

#### **1. `system_config`** (Runs First)  
**Purpose:** Prepares the system environment by ensuring required packages are installed, managing users, and configuring essential dependencies.  

**Ansible Modules Used:**  
- `apt` → Installs necessary system packages  
- `file` → Manages directory structure and permissions  

**Why It Runs First:**  
This role sets up the foundational system requirements, ensuring all subsequent installations and configurations operate without issues.  

---

#### **2. `docker_setup`** (Runs After System Configuration)  
**Purpose:** Installs and configures Docker, including setting up the required runtime environment.  

**Ansible Modules Used:**  
- `apt` → Installs Docker and Docker Compose  
- `user` → Ensures the `vagrant` user has proper Docker permissions  
- `service` → Ensures Docker is running and enabled at boot  

**Why It Runs Second:**  
Docker must be properly installed and configured before any application deployment, as all microservices rely on it for containerization.  

---

#### **3. `app_deployment`** (Runs Last)  
**Purpose:** Clones the application repository, installs dependencies, and runs the app containers.  

**Ansible Modules Used:**  
- `git` → Clones the latest version of the app repository  
- `file` → Ensures required directories exist  
- `command` → Runs `docker-compose up` to start containers  

**Why It Runs Last:**  
The application requires a properly configured Docker runtime, so this role executes once Docker is fully installed and operational.  

---

## **Deployment Testing & Debugging**  

Several validation steps were executed to ensure deployment success:  
- Confirmed all containers were running using `docker ps`  
- Verified backend API functionality using `curl http://localhost:<backend_port>/api/health`  
- Ensured MongoDB was properly linked inside the backend container  
- Checked VM and container logs using `journalctl -u docker`  
- Executed the Ansible playbook manually to validate provisioning steps  

These checks reinforce deployment stability and minimize manual oversight.  

---

## **Git Workflow**  

```plaintext
- Forked and cloned the repository locally  
- Validated application functionality by running it locally  
- Merged `revert-25-master` into `master`  
- Used semantic versioning for each Docker image update  
- Built and pushed microservice Docker images to DockerHub, ensuring proper naming conventions  
- Deployed and tested using Docker Compose, verifying inter-service networking and container functionality  
- Debugged MongoDB connectivity issues to ensure reliable database access  
```

---

## **DockerHub Repository Screenshot**  

![Screenshot displaying DockerHub images of the microservices](./images/DockerHub-Screenshot.png)  

---

