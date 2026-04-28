# Uninstall Coolify

On VPS .

https://coolify.io/docs/get-started/uninstallation

- Using Coolify Web UI stop all projects' containers. 
- Stop database container
- ssh to VPS

1. Stop and Remove Containers

List running containers:
```bash
docker ps
```

Stop containers
```bash
docker stop -t 0 coolify coolify-realtime coolify-db coolify-redis coolify-proxy coolify-sentinel
```

List all containers
```bash
docker container ls -a
```

Remove containers
```bash
docker rm coolify coolify-realtime coolify-db coolify-redis coolify-proxy coolify-sentinel
```

2. Remove Docker Volumes

`docker volume ls` list volumes.

```bash
docker volume rm coolify-db coolify-redis
```
Follow the list, and delete related volumes using the command as above.


3. Remove Docker Network

```bash
docker network rm coolify
```

4. Delete Coolify Data Directory

```bash
rm -rf /data/coolify
```

5. Remove Docker Images

List images
```bash
docker images
```
Remove image
```bash
docker rmi traefik:v3.1
```

Similarly remove other images.

Coolify Successfully Uninstalled .
