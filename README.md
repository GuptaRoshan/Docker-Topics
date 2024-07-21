# Docker Learning Topics Overview

<img src="https://github.com/GuptaRoshan/learning/blob/main/src/notes/assets/docker.jpg" width="80%" height="80%" alt="Docker Concepts">

## 1. Introduction to Docker

### What is Docker?

Docker is a platform for developing, shipping, and running applications in containers. Containers allow developers to package an application with all its dependencies and ship it as a single unit.

### Benefits of using Docker:

- Consistent environment across different stages of development.
- Isolation of applications.
- Efficient resource utilization.

### Key concepts:

- **Images:** Immutable, lightweight, and standalone packages that contain everything needed to run a piece of software, including the code, runtime, libraries, and configuration.
- **Containers:** Running instances of Docker images.
- **Dockerfile:** A text file with instructions to build a Docker image.
- **Docker Hub:** A public registry to find and share container images.

## 2. Installing Docker

### Installing Docker on Windows:

1. Download Docker Desktop from the [Docker website](https://www.docker.com/products/docker-desktop).
2. Follow the installation instructions.
3. Verify the installation:
   ```bash
   docker --version
   ```

### Installing Docker on macOS:

1. Download Docker Desktop for Mac.
2. Follow the installation instructions.
3. Verify the installation:
   ```bash
   docker --version
   ```

### Installing Docker on Linux:

1. Update your existing list of packages:
   ```bash
   sudo apt-get update
   ```
2. Install a few prerequisite packages:
   ```bash
   sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
   ```
3. Add the GPG key for the official Docker repository:
   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```
4. Add the Docker repository to APT sources:
   ```bash
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
   ```
5. Update your package database with the Docker packages from the newly added repo:
   ```bash
   sudo apt-get update
   ```
6. Install Docker:
   ```bash
   sudo apt-get install docker-ce
   ```
7. Verify the installation:
   ```bash
   docker --version
   ```

## 3. Docker Architecture

### Docker Engine

The core component of Docker that creates and manages containers.

### Docker Daemon

Runs on the host machine and listens for Docker API requests to manage Docker objects such as images, containers, networks, and volumes.

### Docker Client

CLI tool that allows users to interact with the Docker daemon.

### Docker Registries

Stores Docker images. Docker Hub is a public registry, but you can also set up private registries.

## 4. Working with Docker Images

### Understanding Docker images

Images are the building blocks of containers. They are created from a set of read-only layers.

### Pulling images from Docker Hub

```bash
    docker pull nginx
```

### Listing images

```bash
    docker images
```

### Removing images

```bash
    docker rmi <image_id>
```

## 5. Creating Docker Images

### Writing a Dockerfile

A Dockerfile is a script containing a series of instructions on how to build a Docker image.

**Example Dockerfile:**

```Dockerfile
    # Use an official Python runtime as a parent image
    FROM python:3.8-slim

    # Set the working directory
    WORKDIR /app

    # Copy the current directory contents into the container at /app
    COPY . /app

    # Install any needed packages specified in requirements.txt
    RUN pip install --no-cache-dir -r requirements.txt

    # Make port 80 available to the world outside this container
    EXPOSE 80

    # Define environment variable
    ENV NAME World

    # Run app.py when the container launches
    CMD ["python", "app.py"]
```

### Building images from a Dockerfile

```bash
    docker build -t my-python-app .
```

### Best practices for writing Dockerfiles

- Keep Dockerfiles small and simple.
- Use .dockerignore to exclude files and directories from the build context.
- Leverage caching by ordering Dockerfile instructions from least to most frequently changed.

### Tagging images

```bash
    docker tag my-python-app myrepo/my-python-app:v1.0
```

## 6. Docker Containers

### Understanding containers

Containers are lightweight, portable, and self-sufficient units that can run any application.

### Running containers

```bash
    docker run -d -p 80:80 --name mynginx nginx
```

### Managing containers (start, stop, restart, remove)

```bash
    docker start mynginx
    docker stop mynginx
    docker restart mynginx
    docker rm mynginx
```

### Inspecting containers

```bash
    docker inspect mynginx
```

### Attaching to and interacting with containers

```bash
    docker exec -it mynginx /bin/bash
```

## 7. Docker Networking

### Overview of Docker networking

Docker networking allows containers to communicate with each other and with the host system.

### Types of Docker networks (bridge, host, overlay, etc.)

- **Bridge:** Default network driver. Containers on the same bridge network can communicate.
- **Host:** Removes network isolation between the container and the Docker host.
- **Overlay:** Enables Swarm services to communicate with each other.

### Creating and managing networks

```bash
    docker network create mynetwork
    docker network ls
    docker network rm mynetwork
```

### Connecting containers to networks

```bash
    docker network connect mynetwork mynginx
```

## 8. Docker Volumes and Storage

### Understanding Docker volumes

Volumes are the preferred mechanism for persisting data generated by and used by Docker containers.

### Creating and managing volumes

```bash
    docker volume create myvolume
    docker volume ls
    docker volume rm myvolume
```

### Mounting volumes in containers

```bash
    docker run -d -p 80:80 --name mynginx -v myvolume:/data nginx
```

### Persistent storage in Docker

Volumes provide persistent storage that is independent of the container lifecycle.

## 9. Docker Compose

### Introduction to Docker Compose

Docker Compose is a tool for defining and running multi-container Docker applications.

### Writing `docker-compose.yml` files

```yaml
    version: '3'
    services:
      web:
        image: nginx
        ports:
          - "80:80"
      db:
        image: postgres
        environment:
          POSTGRES_PASSWORD: example
```

### Building and running multi-container applications with Docker Compose

```bash
    docker-compose up
    docker-compose down
```

### Docker Compose commands

```bash
    docker-compose build
    docker-compose start
    docker-compose stop
```

## 10. Docker Registry

### Understanding Docker Registry

A storage and distribution system for named Docker images.

### Using Docker Hub

Docker Hub is a cloud-based registry service for sharing Docker images.

### Setting up a private Docker Registry

```bash
    docker run -d -p 5000:5000 --name registry registry:2
```

### Pushing and pulling images from a private registry

```bash
    docker tag my-python-app localhost:5000/my-python-app
    docker push localhost:5000/my-python-app
    docker pull localhost:5000/my-python-app
```

## 11. Dockerizing Applications

### Containerizing a simple web application (e.g., Node.js, Python Flask)

**Example Dockerfile for a Node.js app:**

```Dockerfile
    FROM node:14

    WORKDIR /app

    COPY package*.json ./

    RUN npm install

    COPY . .

    EXPOSE 3000

    CMD ["node", "app.js"]
```

### Best practices for containerizing applications

- Use official base images.
- Minimize the number of layers.
- Use multi-stage builds for smaller images.

## 12. Managing Docker Containers

### Using Docker commands for container management

```bash
    docker ps
    docker inspect
    docker logs
    docker stats
```

### Monitoring and logging containers

```bash
    docker logs mynginx
    docker stats mynginx
```

### Using tools like `docker stats` and `docker logs`

```bash
    docker stats
    docker logs
```

## 13. Docker Security

### Security best practices

- Run containers as a non-root user.
- Keep Docker and Docker images up to date.
- Use trusted images.

### Running containers as non-root users

```Dockerfile
    FROM node:14
    USER node
```

### Securing Docker daemon and network

- Use TLS to secure Docker daemon.
- Limit container privileges.
- Use network policies to restrict container communication.

### Scanning images for vulnerabilities

Use tools like Trivy, Clair, and Docker's built-in scanning.

## 14. Advanced Docker Topics

### Docker Swarm and orchestration

Docker Swarm is a container orchestration tool native to Docker.

### Using Docker in CI/CD pipelines

Integrate Docker with CI/CD tools like Jenkins, GitLab CI, and CircleCI.

### Integrating Docker with Kubernetes

Kubernetes is a container orchestration system that extends Docker's capabilities.

### Docker on cloud platforms (AWS, GCP, Azure)

Use Docker with cloud services like AWS ECS, Google Kubernetes Engine, and Azure Container Instances.
