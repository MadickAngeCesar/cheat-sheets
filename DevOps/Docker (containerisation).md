# Docker Cheat Sheet - Beginner to Pro

## Table of Contents
- [Installation](#installation)
- [Basic Commands](#basic-commands)
- [Container Management](#container-management)
- [Image Management](#image-management)
- [Dockerfile](#dockerfile)
- [Docker Compose](#docker-compose)
- [Volumes](#volumes)
- [Networks](#networks)
- [Docker Registry](#docker-registry)
- [Docker Hub](#docker-hub)
- [Multi-stage Builds](#multi-stage-builds)
- [Best Practices](#best-practices)

---

## Installation

### Linux
```bash
# Update package index
sudo apt-get update

# Install dependencies
sudo apt-get install ca-certificates curl gnupg lsb-release

# Add Docker's official GPG key
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Set up repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Verify installation
docker --version
docker compose version

# Run without sudo
sudo usermod -aG docker $USER
newgrp docker
```

### Windows & Mac
```bash
# Download Docker Desktop from:
# https://www.docker.com/products/docker-desktop

# Verify installation
docker --version
docker compose version
```

---

## Basic Commands

### Getting Started
```bash
# Check Docker version
docker --version
docker version

# View system information
docker info

# View help
docker --help
docker run --help

# List all Docker commands
docker
```

---

## Container Management

### Running Containers
```bash
# Run container
docker run nginx

# Run with name
docker run --name my-nginx nginx

# Run in detached mode (background)
docker run -d nginx

# Run with port mapping
docker run -p 8080:80 nginx

# Run with environment variables
docker run -e MY_VAR=value nginx

# Run with volume
docker run -v /host/path:/container/path nginx

# Run interactive container
docker run -it ubuntu bash

# Run and remove after exit
docker run --rm nginx

# Run with custom entrypoint
docker run --entrypoint /bin/sh nginx

# Run with resource limits
docker run --memory="512m" --cpus="1.5" nginx
```

### Container Operations
```bash
# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# List container IDs only
docker ps -q

# Start container
docker start <container_id>

# Stop container
docker stop <container_id>

# Restart container
docker restart <container_id>

# Pause container
docker pause <container_id>
docker unpause <container_id>

# Kill container
docker kill <container_id>

# Remove container
docker rm <container_id>

# Remove running container (force)
docker rm -f <container_id>

# Remove all stopped containers
docker container prune
```

### Container Inspection
```bash
# View container logs
docker logs <container_id>

# Follow logs (real-time)
docker logs -f <container_id>

# Last 100 lines
docker logs --tail 100 <container_id>

# Logs with timestamps
docker logs -t <container_id>

# Inspect container details
docker inspect <container_id>

# View container processes
docker top <container_id>

# View container stats
docker stats <container_id>

# View all containers stats
docker stats

# Execute command in running container
docker exec <container_id> ls /app

# Interactive shell in container
docker exec -it <container_id> bash

# Copy files from container to host
docker cp <container_id>:/path/to/file /host/path

# Copy files from host to container
docker cp /host/path <container_id>:/container/path
```

---

## Image Management

### Working with Images
```bash
# List images
docker images
docker image ls

# Pull image from registry
docker pull nginx
docker pull nginx:1.21

# Search for images
docker search nginx

# Build image from Dockerfile
docker build -t myapp:1.0 .

# Build with no cache
docker build --no-cache -t myapp:1.0 .

# Build with build arguments
docker build --build-arg VERSION=1.0 -t myapp .

# Tag image
docker tag myapp:1.0 myusername/myapp:1.0

# Push image to registry
docker push myusername/myapp:1.0

# Remove image
docker rmi <image_id>

# Remove dangling images
docker image prune

# Remove all unused images
docker image prune -a

# Save image to tar file
docker save -o myapp.tar myapp:1.0

# Load image from tar file
docker load -i myapp.tar

# View image history
docker history <image_id>

# Inspect image
docker inspect <image_id>
```

---

## Dockerfile

### Basic Dockerfile
```dockerfile
# Use base image
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy application files
COPY . .

# Expose port
EXPOSE 3000

# Set environment variables
ENV NODE_ENV=production

# Run command
CMD ["node", "index.js"]
```

### Dockerfile Instructions
```dockerfile
# FROM - Base image
FROM ubuntu:22.04
FROM node:18-alpine AS builder

# LABEL - Metadata
LABEL maintainer="your@email.com"
LABEL version="1.0"

# ENV - Environment variables
ENV APP_HOME=/app
ENV NODE_ENV=production

# WORKDIR - Set working directory
WORKDIR /app

# COPY - Copy files from host to container
COPY package.json .
COPY . /app

# ADD - Similar to COPY but can extract tar files and download URLs
ADD https://example.com/file.tar.gz /tmp/
ADD archive.tar.gz /app

# RUN - Execute commands during build
RUN apt-get update && apt-get install -y curl
RUN npm install

# CMD - Default command (can be overridden)
CMD ["node", "index.js"]
CMD node index.js

# ENTRYPOINT - Command that always runs (harder to override)
ENTRYPOINT ["node"]
CMD ["index.js"]

# EXPOSE - Document port (doesn't actually publish)
EXPOSE 3000
EXPOSE 3000/tcp

# VOLUME - Create mount point
VOLUME ["/data"]

# USER - Set user for subsequent commands
USER node

# ARG - Build-time variables
ARG VERSION=1.0
RUN echo $VERSION

# HEALTHCHECK - Container health check
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost/ || exit 1

# SHELL - Change default shell
SHELL ["/bin/bash", "-c"]

# ONBUILD - Trigger when image is used as base
ONBUILD COPY . /app
ONBUILD RUN npm install
```

### Best Practices Dockerfile
```dockerfile
# Use specific version tags
FROM node:18.17.0-alpine

# Set working directory
WORKDIR /app

# Copy only package files first (cache optimization)
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy application files
COPY . .

# Use non-root user
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001
USER nodejs

# Expose port
EXPOSE 3000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s \
  CMD node healthcheck.js

# Start application
CMD ["node", "index.js"]
```

### Multi-stage Build Example
```dockerfile
# Build stage
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY package*.json ./
EXPOSE 3000
CMD ["node", "dist/index.js"]
```

---

## Docker Compose

### Basic docker-compose.yml
```yaml
version: '3.8'

services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
    environment:
      - NGINX_HOST=localhost
      - NGINX_PORT=80
```

### Complete Example
```yaml
version: '3.8'

services:
  # Web application
  web:
    build:
      context: .
      dockerfile: Dockerfile
      args:
        - NODE_ENV=production
    image: myapp:latest
    container_name: web-app
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/mydb
      - REDIS_URL=redis://cache:6379
    volumes:
      - ./src:/app/src
      - node_modules:/app/node_modules
    depends_on:
      - db
      - cache
    networks:
      - app-network
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s

  # Database
  db:
    image: postgres:15-alpine
    container_name: postgres-db
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
      - POSTGRES_DB=mydb
    volumes:
      - postgres-data:/var/lib/postgresql/data
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
    networks:
      - app-network
    restart: unless-stopped

  # Cache
  cache:
    image: redis:7-alpine
    container_name: redis-cache
    ports:
      - "6379:6379"
    volumes:
      - redis-data:/data
    networks:
      - app-network
    restart: unless-stopped

volumes:
  postgres-data:
  redis-data:
  node_modules:

networks:
  app-network:
    driver: bridge
```

### Docker Compose Commands
```bash
# Start services
docker compose up

# Start in detached mode
docker compose up -d

# Build and start
docker compose up --build

# Start specific services
docker compose up web db

# Stop services
docker compose down

# Stop and remove volumes
docker compose down -v

# View running services
docker compose ps

# View logs
docker compose logs

# Follow logs
docker compose logs -f

# Logs for specific service
docker compose logs -f web

# Execute command in service
docker compose exec web bash

# Run one-off command
docker compose run web npm test

# Scale services
docker compose up -d --scale web=3

# Restart services
docker compose restart

# Pause services
docker compose pause
docker compose unpause

# View service config
docker compose config

# Pull images
docker compose pull

# Build services
docker compose build

# Build without cache
docker compose build --no-cache
```

---

## Volumes

### Volume Commands
```bash
# Create volume
docker volume create myvolume

# List volumes
docker volume ls

# Inspect volume
docker volume inspect myvolume

# Remove volume
docker volume rm myvolume

# Remove all unused volumes
docker volume prune

# Use volume with container
docker run -v myvolume:/app/data nginx
```

### Volume Types
```bash
# Named volume
docker run -v myvolume:/app/data nginx

# Bind mount (host directory)
docker run -v /host/path:/container/path nginx
docker run -v $(pwd):/app nginx

# Anonymous volume
docker run -v /app/data nginx

# Read-only volume
docker run -v myvolume:/app/data:ro nginx

# Volume with specific driver
docker volume create --driver local myvolume
```

---

## Networks

### Network Commands
```bash
# List networks
docker network ls

# Create network
docker network create mynetwork

# Create network with subnet
docker network create --subnet=172.18.0.0/16 mynetwork

# Inspect network
docker network inspect mynetwork

# Connect container to network
docker network connect mynetwork mycontainer

# Disconnect container from network
docker network disconnect mynetwork mycontainer

# Remove network
docker network rm mynetwork

# Remove unused networks
docker network prune
```

### Network Types
```bash
# Bridge network (default)
docker network create --driver bridge mynetwork
docker run --network mynetwork nginx

# Host network (use host's network)
docker run --network host nginx

# None network (no networking)
docker run --network none nginx

# Overlay network (for Swarm)
docker network create --driver overlay mynetwork

# Custom network with options
docker network create \
  --driver bridge \
  --subnet 172.20.0.0/16 \
  --gateway 172.20.0.1 \
  mynetwork
```

---

## Docker Registry

### Working with Registries
```bash
# Login to Docker Hub
docker login

# Login to private registry
docker login registry.example.com

# Logout
docker logout

# Tag image for registry
docker tag myapp:1.0 registry.example.com/myapp:1.0

# Push to registry
docker push registry.example.com/myapp:1.0

# Pull from registry
docker pull registry.example.com/myapp:1.0

# Run local registry
docker run -d -p 5000:5000 --name registry registry:2

# Push to local registry
docker tag myapp:1.0 localhost:5000/myapp:1.0
docker push localhost:5000/myapp:1.0
```

---

## Docker Hub

### Docker Hub Commands
```bash
# Search images
docker search nginx

# Pull image
docker pull nginx:latest

# Pull specific version
docker pull nginx:1.21

# Tag for Docker Hub
docker tag myapp:1.0 username/myapp:1.0

# Push to Docker Hub
docker push username/myapp:1.0

# View image tags on Docker Hub
# Visit: https://hub.docker.com/r/username/myapp/tags
```

---

## Multi-stage Builds

### Node.js Example
```dockerfile
# Build stage
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY package*.json ./
USER node
EXPOSE 3000
CMD ["node", "dist/index.js"]
```

### Go Example
```dockerfile
# Build stage
FROM golang:1.21 AS builder
WORKDIR /app
COPY go.* ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

# Production stage
FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=builder /app/main .
EXPOSE 8080
CMD ["./main"]
```

---

## Best Practices

### Image Optimization
```dockerfile
# ✅ Good: Use specific tags
FROM node:18.17.0-alpine

# ❌ Bad: Using latest tag
FROM node:latest

# ✅ Good: Minimize layers
RUN apt-get update && apt-get install -y \
    curl \
    vim \
    && rm -rf /var/lib/apt/lists/*

# ❌ Bad: Multiple RUN commands
RUN apt-get update
RUN apt-get install -y curl
RUN apt-get install -y vim

# ✅ Good: Copy package files first
COPY package*.json ./
RUN npm install
COPY . .

# ❌ Bad: Copy everything first
COPY . .
RUN npm install

# ✅ Good: Use .dockerignore
# .dockerignore file:
node_modules
npm-debug.log
.git
.env
.DS_Store

# ✅ Good: Use non-root user
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001
USER nodejs

# ✅ Good: Use COPY instead of ADD
COPY . .

# ✅ Good: Multi-stage builds
FROM node:18 AS builder
# ... build steps
FROM node:18-alpine
COPY --from=builder /app/dist ./dist
```

### Security
```bash
# ✅ Scan images for vulnerabilities
docker scan myapp:1.0

# ✅ Use official images
FROM node:18-alpine

# ✅ Keep images updated
docker pull node:18-alpine

# ✅ Don't run as root
USER node

# ✅ Use secrets for sensitive data
docker secret create db_password password.txt
docker service create --secret db_password myapp
```

### Performance
```bash
# ✅ Use volumes for persistent data
docker run -v mydata:/data myapp

# ✅ Limit resources
docker run --memory="512m" --cpus="1.0" myapp

# ✅ Use build cache
docker build -t myapp .

# ✅ Clean up regularly
docker system prune -a
```

### Docker Compose Best Practices
```yaml
# ✅ Good: Use environment variables
services:
  web:
    image: myapp:${VERSION:-latest}
    environment:
      - DB_HOST=${DB_HOST}

# ✅ Good: Use depends_on with healthcheck
services:
  web:
    depends_on:
      db:
        condition: service_healthy
  db:
    healthcheck:
      test: ["CMD", "pg_isready"]

# ✅ Good: Use restart policies
services:
  web:
    restart: unless-stopped

# ✅ Good: Use named volumes
volumes:
  postgres-data:
  redis-data:
```

---

**Pro Tip:** Use multi-stage builds to reduce image size, always use .dockerignore to exclude unnecessary files, leverage build cache by copying package files before source code, and run containers as non-root users for better security!