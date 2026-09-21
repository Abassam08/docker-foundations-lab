# Lab 01 — Docker Basics

## Goal

Confirm Docker Desktop is working and learn the basic Docker architecture.

## Verify Docker

```powershell
docker --version
docker version
docker compose version
docker ps
```

A healthy setup should show both a Docker **Client** and **Server/Engine**.

## Architecture

On Docker Desktop for Windows, the simplified flow is:

```text
Windows
   ↓
Docker Desktop
   ↓
Docker Engine
   ↓
Linux containers
```

## Image vs container

```text
Image
  ↓
Reusable template

Container
  ↓
Instance created from an image
```

List images:

```powershell
docker images
```

List running containers:

```powershell
docker ps
```

List all containers:

```powershell
docker ps -a
```

## Mini exercise

Run Docker's test image:

```powershell
docker run hello-world
```

Then check:

```powershell
docker ps -a
```

The container will normally show as exited. That is expected: it ran its job, printed a message, and stopped.
