# Lab 05 — Docker Networking

## Goal

Create a custom Docker network and prove that containers can discover each other by name.

## View Docker networks

```powershell
docker network ls
```

Typical built-in networks include:

- `bridge`
- `host`
- `none`

## Create a custom bridge network

```powershell
docker network create lab-network
```

Check:

```powershell
docker network ls
```

## Start nginx on the network

```powershell
docker run -d --name web-server --network lab-network nginx
```

## Start a client container on the same network

```powershell
docker run -it --name test-client --network lab-network ubuntu bash
```

Inside Ubuntu:

```bash
apt update
apt install -y curl
curl http://web-server
getent hosts web-server
```

Docker's internal DNS resolves the container name `web-server` to its current container IP.

That means applications should usually connect by name instead of hardcoding container IP addresses.

## Why localhost does not work

Inside `test-client`:

```text
localhost = test-client itself
```

It does **not** mean the nginx container.

To reach nginx, use:

```text
http://web-server
```

because both containers are on the same Docker network.

## Inspect the network

Exit the client and run:

```powershell
docker network inspect lab-network
```

The output shows the attached containers and their internal IP addresses.

## Connect an existing container to a network

Create another nginx container:

```powershell
docker run -d --name isolated-nginx nginx
```

Then attach it:

```powershell
docker network connect lab-network isolated-nginx
```

## Clean up

```powershell
docker stop web-server isolated-nginx
docker rm web-server isolated-nginx test-client
docker network rm lab-network
```
