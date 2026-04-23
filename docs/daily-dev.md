# Daily Operations of Development Environment

After finishing initial development, and production environment setup, we use the development environment as follows.

## Change shell to bash
```bash
ssh-agent bash
```

## Add ssh keys to ssh agent 

```bash
ssh-add ~/.ssh/< vm-user-key >
```

```bash
ssh-add ~/.ssh/coolifyrootkey
```

```bash
ssh-add ~/.ssh/< github-user-key > 
```

Open a second terminal, do those above. Use one to ssh to VM/VPS, the other for local operations.

## Start Virtual Machine
```bash
multipass start coolvm
```
```bash
multipass list
```

Note the IPv4 for later use .

## Check & Note the IP address
```bash
Name                    State             IPv4             Image
coolvm                  Running           10.9.172.90     Ubuntu 24.04 LTS
                                          10.0.1.1
                                          10.0.0.1
                                          10.0.2.1
```
If the IP address is different then the previous run, make necessary changes.

Changes for IP
- /etc/hosts on Developer PC
```bash
.....
10.9.172.90 www.devserver1.my-domain.com
10.9.172.90 traefik.devserver1.my-domain.com coolify.devserver1.my-domain.com 
10.9.172.90 devserver1.my-domain.com
10.9.172.90 devserver1
```
## ssh to Virtual Machine
Me must have the ability to ssh to the Virtual Machine with root user. This is necassary for Coolify.

```bash
ssh root@< virtual-m-ip >
```
or
```bash
ssh root@devserver1.my-domain.com
```
## Update apt packages
```bash
apt update
```

If there are packages to upgrade:
```bash
apt upgrade
```

Reboot if necessary.

## Open Coolify Web Interface
Open Browser: http://`< virtual-m-ip >`:8000/

If you finished domain & TLS setting properly you can use:

https://coolify.devserver1.my-domain.com

Check https://traefik.devserver1.my-domain.com/ping
Check https://traefik.devserver1.my-domain.com/dashboard/
Check https://www.devserver1.my-domain.com
Check https://www.devserver1.my-domain.com/admin

## Development steps
- git-repo-url : https://github.com/< github-username >/< github-app-name >
- payload-app-full-path : /home/< local-user-name >/< local-workspace >/< payload-app-directory >
```bash
cd < payload-app-full-path >
```
```bash
git config list
```
```bash
git status
```
Commit or revert any changes, and make status "nothing to commit, working tree clean".

```bash
git branch --show-current
```
If it is not dev, switch to dev branch

```bash
git switch dev
```
```bash
git branch --show-current
```

- Make changes you see fit for your application.
- Check locally,
  - Mongo URL (public) must be enabled, and DATABASE_URL must be set in .env file.
  ```bash
  cd < payload-app-full-path >
  ```
  - .env file content must be similar to VM Payload CMS Environment Variables
  ```bash
  pnpm install
  ```
  ```bash
  pnpm build
  ```
  - Copy < payload-app-full-path >/public into < payload-app-full-path >/.next/standalone
  - Copy < payload-app-full-path >/.next/static into < payload-app-full-path >/.next/standalone/.next
  ```bash
  node .next/standalone/server.js
  ```
  - Open a web browser, navigate to http://localhost:3000
  - This renders the homepage of Payload.
  - ```CTRL``` + ```C``` on terminal to stop the service

    

#### git commit
```bash
git add .
```
```bash
git commit
```
Enter your message, save & exit

#### git push
```bash
git push origin dev
```

#### Redeploy on VM
- Coolify UI on VM (https://coolify.devserver1.my-domain.com)
  - Project -> < project-name > -> payload -> Redeploy

### Update documentation
- Update documentation about changes.

Continue to [daily-prod](daily-prod.md) if everything is OK.

### TODO: Write docs for Payload CMS editing
### TODO: Add tests

References:
- https://payloadcms.com/