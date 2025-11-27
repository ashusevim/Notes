# Docker

Docker is a platform for developing, shipping, and running applications inside lightweight, portable containers. Containers package code, dependencies, and system tools so your app runs the same everywhere.

- **Image:** A read-only template with instructions for creating a container. Built from a Docker file.
- **Container:** A running instance of an image. Isolated, but shares the host OS kernel.
- **Dockerfile:** A text file with commands to build a Docker image (e.g., `FROM`, `COPY`, `RUN`).
- **Docker Hub:** A cloud-based registry for sharing and downloading images.
- **Docker Compose:** A tool for defining and running multi-container applications using a `docker-compose.yml` file.

1. **Install Docker:** Download Docker Desktop for Windows/Mac or install on Linux.
2. **Build an Image:** Write a Dockerfile, then run `docker build -t <image-name> .`
3. **Run a Container:** Use `docker run <image-name>` to start your app in a container.
4. **Share Images:** Push to Docker Hub with `docker push <image-name>`.
5. **Manage Containers:** List (`docker ps`), stop (`docker stop <container>`), remove (`docker rm <container>`).

# How to write Docker files?

1. start with an docker image
    - we can use `FROM` to specify the starting point
    - **Best practice:** Avoid `latest` tags—use a specific version for predictability and security.
2. set a working directory
    - Use `WORKDIR /app` to set the directory for subsequent commands.
3. copy file
    - Use `COPY` to add your code and files into the image
    - **Tip:** Use a `.dockerignore` file to exclude unnecessary files (like `.git`, `node_modules`).
4. Install dependencies
    - use `RUN` to install packages or dependencies
    - Combine related commands to reduce image layers and speed up builds
5. Set Environment Variables (Optional)
    - Use `ENV` for configuration values that don't contain secrets
    - do not use hard-coded ones
6. Expose the port your app runs on
    - Use `EXPOSE 80` to document which port your app listens on
7. Set the Startup Command
    - Use `CMD` or `ENTRYPOINT` to define how your app starts
8. Run as non-root user

```
RUN useradd -m appuser
USER appuser
```

1. Use Multi-Stage Builds for Production
    - Use `LABEL` to add metadata (e.g., `LABEL maintainer="you@example.com"`).