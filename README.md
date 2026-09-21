# Docker Foundations Lab 🐳

A hands-on beginner guide to Docker based on practical labs.

This repository is designed for anyone who wants to understand Docker by actually using it instead of only reading theory. It covers the same foundation I practiced step by step: containers, images, Dockerfiles, ports, volumes, networking, Docker Compose, Redis, troubleshooting, and cleanup.

## What you will learn

By the end of these labs, you should understand:

- What Docker images and containers are
- How to create, start, stop, inspect, and delete containers
- How port mapping works
- How to enter a running container with `docker exec`
- How to build your own image with a Dockerfile
- Why Docker volumes are important
- How Docker networks let containers communicate
- How Docker Compose manages multi-container applications
- How service-name DNS works inside Compose
- How to safely clean up Docker resources

## Mental model

```text
Dockerfile
    ↓
docker build
    ↓
Image
    ↓
docker run
    ↓
Container
    ├── Ports   → expose an application
    ├── Volumes → persist important data
    └── Network → communicate with other containers

Multiple containers + configuration
                ↓
          Docker Compose
```

## Lab roadmap

| Lab | Topic | What you practice |
|---|---|---|
| 01 | Docker Basics | Check Docker, list images and containers |
| 02 | First Container | Run nginx, map ports, logs, exec, lifecycle |
| 03 | Custom Image | Build a website image using a Dockerfile |
| 04 | Volumes | Persist data outside a container |
| 05 | Networking | Connect containers and use Docker DNS |
| 06 | Docker Compose | Run nginx + Redis as one application |
| 07 | Ubuntu Practice | Quickly create, enter, and delete a Linux lab |

## Prerequisites

- Docker Desktop installed
- Windows PowerShell, Terminal, Bash, or another command line
- Basic comfort using commands such as `cd`, `dir` / `ls`, and `mkdir`

Verify Docker:

```powershell
docker --version
docker version
docker compose version
docker ps
```

## Important idea: image vs container

An **image** is a reusable blueprint.

A **container** is a running or stopped instance created from an image.

Think of it like:

```text
Image = template
Container = running copy of that template
```

You can create many containers from the same image.

## Quick example

Pull nginx:

```powershell
docker pull nginx
```

Run it:

```powershell
docker run -d --name my-nginx -p 8080:80 nginx
```

Open:

```text
http://localhost:8080
```

Then inspect and clean up:

```powershell
docker ps
docker logs my-nginx
docker exec -it my-nginx bash
docker stop my-nginx
docker rm my-nginx
```

## Repository structure

```text
docker-foundations-lab/
├── README.md
├── DOCKER-COMMANDS.md
├── 01-docker-basics/
├── 02-first-container/
├── 03-dockerfile-custom-image/
├── 04-volumes/
├── 05-networking/
├── 06-docker-compose/
└── 07-ubuntu-practice/
```

## Safety note about cleanup

Be careful with:

```powershell
docker compose down -v
```

The `-v` removes Compose-managed volumes. If those volumes contain database or application data, that data can be deleted.

Compare:

```powershell
docker compose down
```

Removes the Compose containers and network but normally keeps named volumes.

```powershell
docker compose down -v
```

Removes the containers, network **and named volumes**.

## Command reference

See [DOCKER-COMMANDS.md](DOCKER-COMMANDS.md) for a categorized cheat sheet of common Docker commands and flags.

---

The goal of this repo is not to memorize every command. It is to understand the lifecycle:

```text
Build → Run → Inspect → Troubleshoot → Persist → Network → Compose → Clean up
```
