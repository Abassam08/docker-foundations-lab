# Docker Command Cheat Sheet

A practical reference for common Docker commands.

## System and help

```powershell
docker --version
docker version
docker info
docker help
docker compose version
```

## Images

List local images:

```powershell
docker images
```

Pull an image:

```powershell
docker pull nginx
docker pull ubuntu
```

Build an image from a Dockerfile:

```powershell
docker build -t my-image .
```

Remove an image:

```powershell
docker rmi my-image
```

## Containers

Show running containers:

```powershell
docker ps
```

Show all containers:

```powershell
docker ps -a
```

Run a container:

```powershell
docker run nginx
```

Run in the background:

```powershell
docker run -d nginx
```

Run interactively:

```powershell
docker run -it ubuntu bash
```

Create a named background Ubuntu lab:

```powershell
docker run -dit --name ubuntu-lab ubuntu bash
```

Enter a running container:

```powershell
docker exec -it ubuntu-lab bash
```

View logs:

```powershell
docker logs container-name
docker logs -f container-name
```

Start, stop, and restart:

```powershell
docker start container-name
docker stop container-name
docker restart container-name
```

Remove a stopped container:

```powershell
docker rm container-name
```

Force-stop and remove:

```powershell
docker rm -f container-name
```

Inspect details:

```powershell
docker inspect container-name
```

## Ports

Map host port 8080 to container port 80:

```powershell
docker run -d -p 8080:80 nginx
```

Remember:

```text
-p HOST_PORT:CONTAINER_PORT
-p 8080:80
```

## Volumes

List volumes:

```powershell
docker volume ls
```

Create a volume:

```powershell
docker volume create docker-lab-data
```

Inspect it:

```powershell
docker volume inspect docker-lab-data
```

Mount it:

```powershell
docker run -it -v docker-lab-data:/data ubuntu bash
```

Remove it:

```powershell
docker volume rm docker-lab-data
```

## Networks

List networks:

```powershell
docker network ls
```

Create a network:

```powershell
docker network create lab-network
```

Run a container on a network:

```powershell
docker run -d --name web-server --network lab-network nginx
```

Inspect a network:

```powershell
docker network inspect lab-network
```

Connect an existing container:

```powershell
docker network connect lab-network container-name
```

Disconnect a container:

```powershell
docker network disconnect lab-network container-name
```

Delete a network:

```powershell
docker network rm lab-network
```

## Docker Compose

Start the application:

```powershell
docker compose up
```

Start in the background:

```powershell
docker compose up -d
```

Build before starting:

```powershell
docker compose up --build
```

See Compose services:

```powershell
docker compose ps
```

View logs:

```powershell
docker compose logs
docker compose logs redis
docker compose logs -f
```

Execute a command inside a service:

```powershell
docker compose exec website bash
docker compose exec redis redis-cli
```

Restart one service:

```powershell
docker compose restart redis
```

Stop and remove the Compose containers/network:

```powershell
docker compose down
```

Remove containers **and volumes**:

```powershell
docker compose down -v
```

⚠️ Be careful with `-v` because persistent data may be deleted.

## Useful flags

| Flag | Meaning | Example |
|---|---|---|
| `-d` | Detached/background mode | `docker run -d nginx` |
| `-i` | Keep STDIN open | Often combined with `-t` |
| `-t` | Allocate a terminal | Often combined with `-i` |
| `-it` | Interactive terminal | `docker exec -it app bash` |
| `-p` | Publish/map a port | `-p 8080:80` |
| `-v` | Mount volume/bind mount | `-v data:/data` |
| `--name` | Give a container a name | `--name ubuntu-lab` |
| `-f` | Force in commands that support it | `docker rm -f app` |
| `--rm` | Delete container automatically when it exits | `docker run --rm hello-world` |

## Quick troubleshooting

Container not running?

```powershell
docker ps -a
docker logs container-name
```

Port already in use?

```powershell
docker ps
```

Use another host port if needed:

```powershell
docker run -d -p 8081:80 nginx
```

Need a shell inside the container?

```powershell
docker exec -it container-name bash
```

If Bash is unavailable:

```powershell
docker exec -it container-name sh
```

Need to understand configuration?

```powershell
docker inspect container-name
docker network inspect network-name
docker volume inspect volume-name
```
