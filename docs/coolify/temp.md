İgnore the following notes for now.

## Create a mount for the project from pc to VM

On VM Coolify creates a directory for this application. The directory is in /data/coolify/applications. Directory name is a created string consisting of number, and letters.

full-application-path-coolify: /data/coolify/applications/< generated-app-directory-name >

Mount a local directory to the Virtual Machine. This is the local copy of the Next.js project.

```bash
multipass mount --gid-map 1000:0 --uid-map 1000:0 --type=classic < workspace-full-path >/< coolify-app-directory > coolvm:< full-application-path-coolify >
```
In 1000:0 1000 is the userid on the developer pc, 0 is root user on VM . You can see your user id, and group id with the id command in a terminal on Controller Pc. If your user id, and group id is different than 1000, then enter that id instead of 1000.
    
Edit this file inside the VM. ssh to VM then edit the source files than save. next.js hot reloads. But it does not hot reloads when edited in Host PC.
   