# Lab 07 — Quick Ubuntu Practice Environment

Sometimes you just want a disposable Linux environment to test commands.

## Create and start Ubuntu

```powershell
docker run -dit --name ubuntu-lab ubuntu bash
```

Flags:

- `-d` = run in the background
- `-i` = keep standard input open
- `-t` = allocate a terminal
- `--name ubuntu-lab` = give the container a readable name

If the Ubuntu image is not already local, Docker pulls it automatically.

## Enter Ubuntu

```powershell
docker exec -it ubuntu-lab bash
```

Try:

```bash
pwd
ls
cat /etc/os-release
```

You can install packages too:

```bash
apt update
apt install -y curl
```

Exit:

```bash
exit
```

## Stop and delete

```powershell
docker stop ubuntu-lab
docker rm ubuntu-lab
```

Or force-stop and delete in one command:

```powershell
docker rm -f ubuntu-lab
```

## Full lifecycle

```text
docker run
    ↓
Ubuntu container
    ↓
docker exec
    ↓
Work inside Linux
    ↓
exit
    ↓
docker stop
    ↓
docker rm
```

This is useful for temporary Linux testing without installing a full VM.
