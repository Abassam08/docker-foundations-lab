# Lab 02 — Your First Real Container

We will run nginx, a small web server.

## Pull nginx

```powershell
docker pull nginx
docker images
```

## Create and start it manually

```powershell
docker create --name my-first-container -p 8080:80 nginx
docker ps -a
docker start my-first-container
docker ps
```

Open:

```text
http://localhost:8080
```

## Port mapping

```text
localhost:8080
      ↓
Host port 8080
      ↓
Container port 80
      ↓
nginx
```

The syntax is:

```text
-p HOST_PORT:CONTAINER_PORT
```

## View logs

```powershell
docker logs my-first-container
```

## Enter the container

```powershell
docker exec -it my-first-container bash
```

Inside:

```bash
pwd
ls
cat /etc/os-release
nginx -v
ls /usr/share/nginx/html
cat /usr/share/nginx/html/index.html
```

Exit:

```bash
exit
```

## Lifecycle

```powershell
docker stop my-first-container
docker start my-first-container
docker restart my-first-container
```

Inspect its configuration:

```powershell
docker inspect my-first-container
```

Clean up:

```powershell
docker stop my-first-container
docker rm my-first-container
```

## Shortcut

Instead of separate `create` and `start` commands:

```powershell
docker run -d --name my-first-container -p 8080:80 nginx
```
