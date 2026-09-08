# New Format Application - Docker Deployment Guide

## Prerequisites

Make sure the following are installed:

- Docker
- Git

Verify:

```bash
docker --version
git --version
```

---

# Step 1: Clone the Repository

```bash
git clone https://github.com/Yashwanth-Gowda-M/new_format_application.git
```

---

# Step 2: Navigate to the Project

```bash
cd new_format_application
```

---

# Step 3: Build Docker Images

Build the backend image:

```bash
docker build -t assignment-backend:v1 -f backend/Dockerfile .
```

Build the frontend image:

```bash
docker build -t assignment-frontend:v1 ./frontend
```

---

# Step 4: Create Docker Network

```bash
docker network create assignment-net
```

---

# Step 5: Start Redis Container

```bash
docker run -d \
  --name assignment-redis \
  --network assignment-net \
  redis:7-alpine
```

---

# Step 6: Start Backend Container

> Replace `XXXXXXXXXXXXXXXXXX` with your Supabase database password.

```bash
docker run -d \
  --name assignment-backend \
  --network assignment-net \
  -p 5000:5000 \
  -e DATABASE_URL="postgresql://postgres.lpnivlbrjbxzcxfmwpon:XXXXXXXXXXXXXXXXXX@aws-0-ap-northeast-2.pooler.supabase.com:5432/postgres?sslmode=require" \
  -e REDIS_URL="redis://assignment-redis:6379/0" \
  assignment-backend:v1
```

---

# Step 7: Start Frontend Container

```bash
docker run -d \
  --name assignment-frontend \
  --network assignment-net \
  -p 80:80 \
  assignment-frontend:v1
```

---

# Step 8: Verify Container Logs

Backend logs:

```bash
docker logs assignment-backend
```

Frontend logs:

```bash
docker logs assignment-frontend
```

Redis logs:

```bash
docker logs assignment-redis
```

---

# Step 9: Verify Redis Cache

Open the Redis CLI monitor:

```bash
docker exec -it assignment-redis redis-cli MONITOR
```

In another terminal, access the application or trigger an API that uses caching.

You should see Redis commands similar to:

```text
GET portal:dashboard:admin:global
```

---

# Useful Docker Commands

List running containers:

```bash
docker ps
```

List all containers:

```bash
docker ps -a
```

List Docker images:

```bash
docker images
```

Stop all application containers:

```bash
docker stop assignment-frontend assignment-backend assignment-redis
```

Remove containers:

```bash
docker rm assignment-frontend assignment-backend assignment-redis
```

Remove Docker network:

```bash
docker network rm assignment-net
```

Remove Docker images:

```bash
docker rmi assignment-frontend:v1 assignment-backend:v1
```

---

# Application URLs

Frontend:

```
http://localhost
```

Backend API:

```
http://localhost:5000
```

---

# Project Repository

https://github.com/Yashwanth-Gowda-M/new_format_application
