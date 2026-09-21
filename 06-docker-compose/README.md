# Lab 06 — Docker Compose + Redis

## Goal

Use one Compose file to manage a multi-container application.

This lab combines:

- A custom nginx website
- Redis
- Port mapping
- A named volume
- An automatically created Compose network
- Service-name DNS

## Project structure

```text
06-docker-compose/
├── Dockerfile
├── index.html
└── compose.yaml
```

## Compose file

```yaml
services:

  website:
    build: .
    ports:
      - "8080:80"

  redis:
    image: redis:alpine
    volumes:
      - redis-data:/data

volumes:
  redis-data:
```

## Start everything

Foreground mode:

```powershell
docker compose up
```

Background mode:

```powershell
docker compose up -d
```

Open:

```text
http://localhost:8080
```

## What Compose automatically handles

```text
compose.yaml
    ↓
Build website image
Create website container
Pull Redis image
Create Redis container
Create default network
Create Redis volume
Map port 8080 → 80
Start the application
```

## Inspect the services

```powershell
docker compose ps
docker ps
```

Compose containers are still normal Docker containers. Compose simply manages them as one project.

## Work with Redis

Open the Redis CLI:

```powershell
docker compose exec redis redis-cli
```

Inside Redis:

```text
PING
SET student "Ahmed"
GET student
```

Expected:

```text
PONG
OK
"Ahmed"
```

Exit Redis:

```text
exit
```

## Test volume persistence

Restart Redis:

```powershell
docker compose restart redis
docker compose exec redis redis-cli
```

Then:

```text
GET student
```

The value should still exist.

A stronger test is to remove the Redis container completely:

```powershell
docker compose rm -sf redis
docker compose ps
docker compose up -d redis
docker compose exec redis redis-cli
```

Then:

```text
GET student
```

The value should still exist because the data lives in the named volume, not only in the container.

### What does `-sf` mean?

For:

```powershell
docker compose rm -sf redis
```

- `-s` = stop the container before removing it
- `-f` = force / do not prompt for confirmation

## Compose networking

List networks:

```powershell
docker network ls
```

You should see a network similar to:

```text
docker-foundations-lab_default
```

or a name based on the directory/project name.

Inspect it:

```powershell
docker network inspect <compose-network-name>
```

Both `website` and `redis` should appear.

Enter the website service:

```powershell
docker compose exec website bash
```

Inside:

```bash
getent hosts redis
```

Docker resolves the Compose service name `redis` to Redis's internal container IP.

That is why applications in Compose commonly use service names such as:

```text
redis:6379
database:5432
api:3000
```

instead of hardcoded container IPs.

## Docker vs Docker Compose

Direct Docker:

```powershell
docker exec -it docker-learning-website-1 bash
```

Compose:

```powershell
docker compose exec website bash
```

Compose knows that `website` refers to the service defined in `compose.yaml`.

## Stop the application

```powershell
docker compose down
```

This normally keeps named volumes.

Be careful with:

```powershell
docker compose down -v
```

The `-v` also removes the project's named volumes and can delete persistent data.
