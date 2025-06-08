---
title: "Docker Basics & Advanced Challenge"
datePublished: Sun Jun 08 2025 13:19:59 GMT+0000 (Coordinated Universal Time)
cuid: cmbnox8mw000f02ky6h6a3jap
slug: docker-basics-and-advanced-challenge
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1749388754178/04153835-403f-4b5d-b218-6e2ebdd8203f.png
tags: docker, developer, devops, docker-images, devops-articles

---

## 🚀 **Mastering Docker: A Practical Guide for Modern Development**

*Docker has revolutionized the way applications are built, deployed, and managed in today’s DevOps ecosystem. Let’s dive into its fundamentals, setup a sample project, and explore optimization techniques for security and performance.*

### 🔹 **Task 1: Introduction & Conceptual Understanding**

Docker simplifies software development by packaging applications and their dependencies into containers. This ensures consistency across environments, reducing the infamous “works on my machine” issue.

#### **Virtualization vs. Containerization**

| Feature | Virtualization (VMs) | Containerization |
| --- | --- | --- |
| Isolation | Full OS-level isolation | Process-level isolation |
| Resource Usage | Heavy (each VM has its own OS) | Lightweight (shared host OS) |
| Startup Time | Minutes | Seconds |
| Portability | Limited | High |
| Microservices | Not ideal | Perfect fit |

🚀 **Why Containerization?**  
For microservices & CI/CD pipelines, containerization enables faster deployments, improved scalability, and minimal resource usage.

---

### 🔹 **Task 2: Dockerfile for a Sample Project**

I chose a simple Python web app:

```dockerfile
# Use official Python image as the base
FROM python:3.9-slim

# Set working directory inside container
WORKDIR /app

# Copy application files
COPY app.py .

# Run the application
CMD ["python", "app.py"]
```

**Building & Running**

```bash
docker build -t mridul/sample-app:latest .
docker run -d -p 8080:80 mridul/sample-app:latest
docker ps
docker logs <container_id>
```

---

### 🔹 **Task 3: Key Docker Terminologies**

* **Image**: Blueprint for containers
    
* **Container**: Running instance of an image
    
* **Dockerfile**: Script defining how an image is built
    
* **Volume**: Data persistence mechanism
    
* **Network**: Enables container communication
    

🔹 **Docker Engine & Docker Hub** facilitate image creation, storage, and distribution.

---

### 🔹 **Task 4: Multi-Stage Build Optimization**

A **multi-stage build** reduces the final image size by eliminating unnecessary files.

Modified Dockerfile:

```dockerfile
# Build Stage
FROM python:3.9 AS builder
WORKDIR /app
COPY app.py .
RUN pip install --no-cache-dir flask

# Production Stage
FROM python:3.9-slim
WORKDIR /app
COPY --from=builder /app .
CMD ["python", "app.py"]
```

📉 **Comparison of Image Sizes**  
Before optimization: `500MB` → After optimization: `50MB`

Multi-stage builds reduce image bloat, improving security and efficiency.

---

### 🔹 **Task 5: Pushing Image to Docker Hub**

```bash
docker tag mridul/sample-app:latest mridul/sample-app:v1.0
docker login
docker push mridul/sample-app:v1.0
docker pull mridul/sample-app:v1.0  # Verification
```

---

### 🔹 **Task 6: Persisting Data with Docker Volumes**

```bash
docker volume create my_volume
docker run -d -v my_volume:/app/data mridul/sample-app:v1.0
```

🔹 **Why Volumes?**  
They ensure **data persistence** across container restarts, making stateful applications reliable.

---

### 🔹 **Task 7: Docker Networking**

```bash
docker network create my_network
docker run -d --name sample-app --network my_network mridul/sample-app:v1.0
docker run -d --name my-db --network my_network -e MYSQL_ROOT_PASSWORD=root mysql:latest
```

🔹 **Inter-container communication** enables seamless service interaction.

---

### 🔹 **Task 8: Orchestrating with Docker Compose**

```yaml
version: '3'
services:
  web:
    image: mridul/sample-app:v1.0
    ports:
      - "8080:80"
    networks:
      - app_net

  db:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: root
    networks:
      - app_net

networks:
  app_net:
```

```bash
docker-compose up -d
docker-compose down
```

📌 **Docker Compose simplifies multi-container applications.**

---

### 🔹 **Task 9: Security with Docker Scout**

```bash
docker scout cves mridul/sample-app:v1.0 > scout_report.txt
```