## Install nvm and node 

On Controller PC
- Add your ssh keys

- Connect to the computer you want to install nvm and node (Local, VM or VPS)

### Install nvm 

https://nodejs.org/en/download

New: Get Node.js v24.16.0(LTS) for Linux using nvm with pnpm

On Developer PC

```bash
cd ~
```
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash
```

End terminal session. Close all open terminals. Open a new terminal. 

```bash
command -v nvm
```

If the result is "nvm" then nvm installation is complete. Continue with [Install Node](#install-node)


### Install Node

```bash
$ nvm install v24.16.0
```

```bash
$ nvm use v24.16.0
```

```bash
$ nvm alias default v24.16.0
```

```bash
$ node -v
```
```bash
v24.16.0
```

```bash
$ nvm which v24.16.0
```
```bash
/<user-home-directory>/.config/nvm/versions/node/v24.16.0/bin/node
```

### Install pnpm

https://pnpm.io/installation


```bash
npm install --global corepack@latest
```

```bash
corepack enable
```

```bash
corepack enable pnpm
```

Continue with [Create a Payload CMS Application](../development.md#create-a-payload-cms-application)

-----
References:
- Node Version Manager: https://github.com/nvm-sh/nvm
- https://pnpm.io/docker
