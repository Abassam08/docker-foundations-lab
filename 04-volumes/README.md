# Lab 04 — Docker Volumes

## Goal

Understand why container data and persistent data should be treated differently.

Containers are disposable. Important data should usually live outside the container's writable filesystem.

## Create a volume

```powershell
docker volume ls
docker volume create docker-lab-data
docker volume inspect docker-lab-data
```

## Attach it to Ubuntu

```powershell
docker run -it --name volume-test -v docker-lab-data:/data ubuntu bash
```

Inside the container:

```bash
cd /data
echo "This data belongs to the Docker volume" > test.txt
cat test.txt
ls
exit
```

## Delete the container

```powershell
docker stop volume-test
docker rm volume-test
```

The container is gone, but the named volume still exists:

```powershell
docker volume ls
```

## Attach the same volume to a new container

```powershell
docker run -it --name volume-test-2 -v docker-lab-data:/data ubuntu bash
```

Inside:

```bash
ls /data
cat /data/test.txt
exit
```

The file should still exist.

## What this proves

```text
Container filesystem
        ↓
Tied to the container

Named volume
        ↓
Independent of the container lifecycle
```

A container can be deleted and recreated while the volume survives.

## Clean up

```powershell
docker stop volume-test-2
docker rm volume-test-2
docker volume rm docker-lab-data
```

Only remove a volume when you are sure you no longer need its data.
