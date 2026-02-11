<img src="docker.png">

# Docker Hands-On Collection

A curated, hands-on Docker learning repository organized by topic.  
Each folder contains a focused lab with commands, notes, and the required artifacts (e.g., `Dockerfile`, `compose.yml`, app source code) to reproduce the exercise.

This repo is designed to be **practical**, **repeatable**, and easy to navigate—ideal for revising Docker fundamentals and showcasing real hands-on work.

---

## Repository Structure

Each directory follows the naming pattern:

- `docker-XX-<topic-name>`

Inside a topic folder you may find:

- `README.md` — step-by-step lab instructions and explanations
- `Dockerfile` / `Dockerfile-alpine` — image build examples
- `compose.yml` — Docker Compose labs
- Example application code (Python, etc.)
- Supporting files (e.g., `requirements.txt`, scripts)

---

## Topics (Labs Overview)

### 01) Installing Docker on EC2 (Amazon Linux)

**Folder:** `docker-01-installing-on-ec2-linux2`  
Covers installing Docker on an AWS EC2 instance, enabling the service, verifying the setup, and running first containers.

What you’ll typically practice:

- Install / start Docker
- Enable Docker at boot
- Run and validate test containers

---

### 02) Container Basic Operations

**Folder:** `docker-02-container-basic-operations`  
Core container lifecycle and daily commands.

What you’ll typically practice:

- `docker run`, `ps`, `stop`, `start`, `rm`
- Interactive vs detached containers
- Naming containers, exposing ports
- `exec` into running containers

---

### 03) Handling Volumes

**Folder:** `docker-03-handling-volumes`  
Data persistence and sharing data between containers using Docker volumes.

What you’ll typically practice:

- Create and inspect volumes
- Bind mounts vs named volumes
- Persistence across container recreation
- Sharing a volume between multiple containers

---

### 04) Docker Networking

**Folder:** `docker-04-network`  
How containers communicate and how networks are created and used.

What you’ll typically practice:

- Default bridge network behavior
- Custom bridge networks
- Container-to-container communication by name
- Port publishing concepts

---

### 05) Image Basic Operations

**Folder:** `docker-05-image-basic-operations`  
Working with images: building, tagging, listing, and cleaning up.

What you’ll typically practice:

- `docker build`, `tag`, `images`, `rmi`
- Image layers and caching basics
- Understanding `Dockerfile` fundamentals

---

### 06) Docker Compose Operations

**Folder:** `docker-06-compose-operations`  
Multi-container setups using Docker Compose.

What you’ll typically practice:

- `docker compose up/down`
- Services, networks, volumes in Compose
- Environment variables in Compose
- Running app stacks locally

---

### 07) Dockerize a To-Do API (Python)

**Folder:** `docker-07-dockerize-to-do-app-on-python`  
Containerizing a small Python-based API application.

What you’ll typically practice:

- Writing a production-friendly Dockerfile
- Installing dependencies cleanly (`requirements.txt`)
- Running the app with Compose (if included)
- Basic runtime configuration

---

### 08) Difference Between `exec` and `run`

**Folder:** `docker-08-difference-between-exec-for...`  
A focused lab explaining the real-world difference between:

- `docker run` (creates + starts a new container)
- `docker exec` (runs a command inside an existing container)

---

### 09) Build Image with ENV and ARG

**Folder:** `docker-09-build-image-with-ENV-and-A...`  
How to pass configuration at build-time vs runtime.

What you’ll typically practice:

- `ARG` for build-time values
- `ENV` for runtime environment variables
- Best practices for configuration and defaults

---

### 10) Logs & Process Insights

**Folder:** `docker-10-docker-logs-command_and-e...`  
Observability basics for containers.

What you’ll typically practice:

- `docker logs` (follow / tail)
- Understanding container stdout/stderr
- Practical debugging workflow

---

### 11) Multi-Stage Builds

**Folder:** `docker-11-multi-stage-builds`  
Build smaller, cleaner images using multi-stage builds.

What you’ll typically practice:

- Separate build vs runtime stages
- Reduce final image size
- Improve security and performance by minimizing layers and dependencies

---

### 12) Monitoring & Container Resource Usage

**Folder:** `docker-12-docker_top_stats-cp-diff-com...`  
Practical tools for inspecting container behavior and resources.

What you’ll typically practice:

- `docker top` vs `docker stats`
- Copy files (`docker cp`)
- Container diffs (`docker diff`)
- When and why to use each command

---

### 13) Commit / Export / Import

**Folder:** `docker-13-docker-commit-export-impor...`  
Working with container state and moving container images/data across environments.

What you’ll typically practice:

- `docker commit` (snapshot container into image)
- `docker export` / `import`
- Basic portability concepts and caveats

---

### Office Hours / Extras

**Folder:** `docker-office-hours`  
Additional notes, troubleshooting, or extra exercises collected during sessions.

---

## How to Use This Repo

1. Pick a folder topic (e.g., `docker-06-compose-operations`)
2. Read the local `README.md`
3. Run the commands and reproduce the outcome
4. Modify the examples to deepen understanding

> Tip: Treat each folder like an independent mini-lab.

---

## Requirements

- Docker Engine installed (Linux / macOS / Windows)
- Optional: Docker Compose v2 (`docker compose ...`)

---

## Notes

- Folder contents may vary by lab: some are command-based, some include a small app and a Dockerfile/Compose stack.
- The goal is reproducible learning rather than a single monolithic project.

---
