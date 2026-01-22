## Install nvm and node on Controller PC or VPS

### Install nvm 

https://nodejs.org/en/download

Get Node.js v22.21.1(LTS) for Linux using nvm with pnpm


```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
```

End terminal session. Close all open terminals. Open a new terminal. Node runs on bash .
```bash
ssh-agent bash
```
Add your ssh keys

```bash
ssh-add <vps-user-key>
```

```bash
ssh-add <vm-user-key>
```

ssh to VM or VPS
```bash
ssh <vps-user-name>@<vps-ip-address> -p <vps-ssh-port>
```
or
```bash
ssh <vm-user-name>@<virtual-m-ip> -p <vm-ssh-port>
```

```bash
command -v nvm
```
If the result is "nvm" then nvm installation is complete.

### Install Node

```bash
nvm install 22
```

```bash
nvm use 22
```

```bash
nvm alias default 22
```

```bash
node -v
v22.21.1
```

```bash
nvm which v22.21.1
/root/.nvm/versions/node/v22.21.1/bin/node
```

### Install pnpm

https://pnpm.io/installation


```bash
npm install --global corepack@latest
```

Prompts for an npm update 11.7.0

```bash
npm install -g npm@11.7.0
```

```bash
corepack enable pnpm
```

```bash
pnpm -v
```

```
! Corepack is about to download https://registry.npmjs.org/pnpm/-/pnpm-10.27.0.tgz
? Do you want to continue? [Y/n] 
```
[ENTER] to continue

```bash
10.27.0
```

Continue with
- [Set swap](set_swap.md)



-----
References:
- Node Version Manager: https://github.com/nvm-sh/nvm
- https://pnpm.io/docker
