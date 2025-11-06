# 🐋 Docker Notes 🐋

**Purpose:**  
This folder contains my reference notes, achievements, and key commands from my Docker learning journey. It covers everything from basic containerization to multi-container orchestration with Docker Compose.

---

## 🐳 Docker Learning Achievements

### **Core Mastery**
✅ Understood containerization fundamentals — isolation, consistency, and resource efficiency  
✅ Learned Docker architecture and components: *Docker Engine, Hub, Compose*  
✅ Mastered Dockerfile creation and essential build commands  

### **Hands-On Development**
✅ Built and containerized a real **Flask web application**  
✅ Created production-ready **Dockerfiles** from scratch  
✅ Successfully ran containers in **detached mode** with proper **port mapping**  
✅ Managed full container lifecycle *build, run, stop, monitor*  

### **Advanced Skills**
✅ Understood **Docker networking** (bridge, host, and none)  
✅ Used **Docker Compose** for multi-container orchestration (Flask + Redis setup)  
✅ Adopted **DevOps best practices** for reproducible environments  

### **Professional Impact**  
✅ Enabled **rapid deployment** and easier scaling  
✅ Improved **team collaboration** and onboarding consistency  
✅ Prepared for **CI/CD pipeline** integration  

---

## ⚙️ Important Docker Commands

| Command | Description |
|----------|--------------|
| `docker ps` | List running containers |
| `docker images` | List all local images |
| `docker inspect <container>` | View detailed container configuration |
| `docker rm <container>` | Remove container |
| `docker rmi <image>` | Remove image |
| `docker system prune -af` | Delete all unused containers, images, networks, and volumes |
| `docker build -t myapp:latest .` | Build an image from the Dockerfile in the current directory |
| `docker run -d -p 5000:5000 myapp` | Run a container in detached mode with port mapping |
| `docker exec -it <container> bash` | Open a shell inside a running container |
| `docker logs <container>` | View container logs |
| `docker stop <container>` | Stop a running container |
| `docker start <container>` | Start a stopped container |

---

## 🐙 Docker Compose Commands

| Command | Description |
|----------|--------------|
| `docker-compose up -d` | Build and start all services (detached mode) |
| `docker-compose down` | Stop and remove containers, networks, and volumes |
| `docker-compose ps` | Show running services |
| `docker-compose logs <service>` | View service logs |
| `docker-compose exec <service> bash` | Open a shell inside a running service |
| `docker-compose build --no-cache` | Rebuild images without cache |

---

## 🧩 Key Concepts to Remember

| Concept | Description |
|----------|--------------|
| **Image** | A read-only blueprint containing application code and dependencies |
| **Container** | A running instance of an image |
| **Volume** | Persistent storage mechanism for containers |
| **Network** | Allows containers to communicate with each other |
| **Dockerfile** | Script containing build instructions for an image |
| **Docker Compose** | Tool to define and run multi-container applications |

---

## 🧰 Useful Maintenance Commands

| Purpose | Command |
|----------|----------|
| Clean up everything unused | `docker system prune -af` |
| Remove all stopped containers | `docker container prune` |
| Remove all unused images | `docker image prune -a` |
| List networks | `docker network ls` |
| List volumes | `docker volume ls` |

---

## 🔗 Helpful References

- [Official Docker Docs](https://docs.docker.com/)
- [Docker Compose Reference](https://docs.docker.com/compose/)
- [Dockerfile Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [Play with Docker (Online Lab)](https://labs.play-with-docker.com/)
- [Docker Hub](https://hub.docker.com/)

---

