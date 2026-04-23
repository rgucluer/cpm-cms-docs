# VPS Disk Space Problem

On VPS

```bash
docker builder prune
```
```bash
WARNING! This will remove all dangling build cache. Are you sure you want to continue? [y/N]
```
<kbd>y</kbd> <kbd>ENTER</kbd>

```bash
reboot
```

Wait nearly a minute, than ssh to VPS, and continue.

```bash
docker ps
```

If all containers are healthy check https://coolify.`< domain-name >`

---

If problem continues,

```bash
cd /
```

```bash
ncdu
```

ncdu ( NCurses Disk Usage ) utility provides easy listing of file and directory sizes

Use ncdu to find cause of disk usage. Delete where possible

---

You can see a message like the following when you log in to your VPS

```bash
 => / is using 97.0% of 37.23GB
```

Or during apt update we can get a message like:
```bash
W: Failed to fetch https://mirror.....  Error writing to file - write (28: No space left on device) [IP:..... 443]

```

```bash
df -h
```
```bash
......
/dev/sda1        38G   37G     0 100% /
......
```

---

Node related files must not exist in root `/` directory. Delete files like the following if exists:
- /package.json
- /pnpm-lock.yaml
- /node_modules/

---

```bash
docker volume list
```

Delete unused volumes
```bash
docker volume rm < volume-name >
```

```bash
docker ps
```

Find payload service copy it's name

```bash
docker stop < payload_container_name >
```

Stop Coolify related containers
```bash
docker stop coolify-sentinel
docker stop coolify
docker stop coolify-db
docker stop coolify-realtime
docker stop coolify-redis
docker stop < generated_app_id >
docker stop < generated_app_id >-proxy
docker stop coolify-proxy

```

---

Delete unused Docker images

```bash
docker images
```

```bash
docker image rm < image-name >
```

---


