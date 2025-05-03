 

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
    - frontend_setup
    - backend_setup
    - mongo_setup
```

Originally, a **single role** (`app_deployment`) handled container setup inside the VM. While functional, this lacked modularity, making individual microservice management difficult.  

To improve **maintainability, debugging efficiency, and scalability**, the deployment workflow was **refactored into distinct roles**:  

- **`frontend_setup`** → Deploys the frontend container (`yolo-client`).  
- **`backend_setup`** → Manages backend deployment (`yolo-backend`).  
- **`mongo_setup`** → Ensures MongoDB (`app-ip-mongo`) is correctly configured and persistent.  

---

### **Ansible Modules Applied Within Each Role**  

Each role uses specific **Ansible modules** to accomplish its tasks efficiently. The table below provides an overview of the modules used and their respective roles.

| **Ansible Module**  | **Purpose**  | **Roles Applied In** |
|---------------------|-------------|----------------------|
| `apt`              | Installs necessary system packages | `system_config`, `docker_setup` |
| `file`             | Ensures required directories exist | `system_config` |
| `git`              | Clones the application repository | `system_config` |
| `user`             | Manages user permissions (adds vagrant user to the Docker group) | `docker_setup` |
| `systemd`          | Ensures services (like Docker) are started and enabled at boot | `docker_setup`, `system_config` |
| `command`          | Runs shell commands (e.g., starting containers using `docker-compose`) | `frontend_setup`, `backend_setup`, `mongo_setup` |
| `debug`            | Displays messages during execution for verification | `docker_setup` |

---

### **Role Breakdown and Justification**  

#### **1. `system_config`** (Runs First)  
**Purpose:** Prepares the system environment by ensuring required packages are installed, managing users, and configuring essential dependencies.  

- Uses `apt` to install essential packages like Git and Curl.  
- Uses `file` to create necessary directories for application deployment.  
- Uses `git` to clone the application repository.  

---

#### **2. `docker_setup`** (Runs After System Configuration)  
**Purpose:** Installs and configures Docker to ensure a proper runtime environment.  

- Uses `apt` to install Docker.  
- Uses `systemd` to enable and start the Docker service.  
- Uses `user` to add the `vagrant` user to the Docker group.  
- Uses `debug` to confirm the installed Docker version.  

---

#### **3. Refactored Application Deployment**  
The previous **`app_deployment`** role was replaced with modular roles for **granular control over each service**.

- **`frontend_setup`** → Uses `command` to start the **frontend container (`yolo-client`)**.  
- **`backend_setup`** → Uses `command` to start the **backend container (`yolo-backend`)**.  
- **`mongo_setup`** → Uses `command` to start the **MongoDB container (`app-ip-mongo`)**.  

Each of these roles ensures that **service-specific provisioning is handled cleanly**, making debugging easier and reducing the risk of breaking dependencies across microservices.

---

### **Deployment Testing & Debugging**  

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

