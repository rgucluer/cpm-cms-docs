## Install nvm and node on Controller PC or VPS

### Install nvm 

https://nodejs.org/en/download

Get Node.js v22.22.1(LTS) for Linux using nvm with pnpm

ssh to VM or VPS if you are installing Node on VM on VPS, otherwise skip this step (on Developer PC)
```bash
ssh <vps-user-name>@<vps-ip-address> -p <vps-ssh-port>
```
or
```bash
ssh <vm-user-name>@<virtual-m-ip> -p <vm-ssh-port>
```

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash
```

End terminal session. Close all open terminals. Open a new terminal. 

```bash
command -v nvm
```

If the result is "nvm" then nvm installation is complete. Continue with [Install Node](#install-node)


Node runs on bash . If command -v nvm comman does not work, then run the following command first. No need to run this command on Ubuntu.

On Controller PC
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

### Install Node

```bash
nvm install v22.22.1
```

```bash
nvm use v22.22.1
```

```bash
nvm alias default v22.22.1
```

```bash
node -v
v22.22.1
```

```bash
nvm which v22.22.1
/root/.nvm/versions/node/v22.22.0/bin/node
```

### Install pnpm

https://pnpm.io/installation


```bash
npm install --global corepack@latest
```

Prompts for an npm update 11.12.0

```bash
npm install -g npm@11.12.0
```

```bash
corepack enable pnpm
```

```bash
pnpm -v
```

```bash
10.32.3
```

Continue with
- [Set swap](../set_swap.md)



-----
References:
- Node Version Manager: https://github.com/nvm-sh/nvm
- https://pnpm.io/docker
