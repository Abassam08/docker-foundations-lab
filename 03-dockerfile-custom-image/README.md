# Lab 03 — Build Your Own Docker Image

## Goal

Create a custom nginx website and package it into your own Docker image.

## Files

This lab uses:

```text
Dockerfile
index.html
```

## Dockerfile

```dockerfile
FROM nginx:latest

COPY index.html /usr/share/nginx/html/index.html
```

Explanation:

- `FROM nginx:latest` starts from the nginx image.
- `COPY` places our HTML file inside the image.

## Build

From this directory:

```powershell
docker build -t ahmed-docker-site .
```

The final `.` means: use the current folder as the build context.

Check the image:

```powershell
docker images
```

## Run

```powershell
docker run -d -p 8080:80 --name ahmed-site ahmed-docker-site
```

Open:

```text
http://localhost:8080
```

## Verify the copied file

```powershell
docker exec -it ahmed-site bash
```

Then:

```bash
cat /usr/share/nginx/html/index.html
exit
```

## Rebuild after editing

Edit `index.html`, then:

```powershell
docker build -t ahmed-docker-site .
docker stop ahmed-site
docker rm ahmed-site
docker run -d -p 8080:80 --name ahmed-site ahmed-docker-site
```

This reinforces:

```text
Source files
    ↓
Dockerfile
    ↓
docker build
    ↓
Image
    ↓
docker run
    ↓
Container
```
