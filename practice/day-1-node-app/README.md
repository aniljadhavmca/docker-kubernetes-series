
# 🚀 Node.js Hello World with Docker (Complete Guide)

This project shows how to:
- Create a simple Node.js "Hello World" app
- Dockerize the application
- Build and run a container

---

# 📌 1. Prerequisites

- Basic terminal knowledge
- Docker installed

---

# 🐳 2. Install Docker

## Mac / Windows

Download Docker Desktop:
https://www.docker.com/products/docker-desktop

### Verify Installation
```bash
docker --version
docker run hello-world

sudo apt update
sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker

```bash
mkdir hello-node
cd hello-node
```

## Build Docker Image
```bash
docker build -t hello-node .
```

## Run Docker Container
```bash
docker run -p 3000:3000 hello-node
```

## Open browser:
your-ip:3000

## List running containers
```bash
docker ps
```

List all containers

```bash
docker ps -a
```

```bash
docker stop <container_id>
```

## Run in background
```bash
docker run -d -p 3000:3000 hello-node
```
