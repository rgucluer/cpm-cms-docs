# Install nvm and node 

On Controller PC
- Add your ssh keys
- When you see a $ sign in a terminal, it means enter command after it. I will use this notation from now on. Will fix the documentation before this point as soon as possible.
  - Done this to differentiate between result displaying terminals from action needed terminals.

## Install nvm 

https://nodejs.org/en/download

Get Node.js v24.18.0(LTS) for Linux using nvm with pnpm

On Developer PC

```bash
$ cd ~
```
```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.6/install.sh | bash
```

```bash
$ \. "$HOME/.nvm/nvm.sh"
```

```bash
$ command -v nvm
```

If the result is "nvm" then nvm installation is complete. 

## Install Node

```bash
$ nvm install v24.18.0
```

```bash
$ nvm use v24.18.0
```

```bash
$ nvm alias default v24.18.0
```

```bash
$ node -v
```
```bash
v24.18.0
```

## Install pnpm

https://pnpm.io/installation


```bash
$ npm install --global corepack@latest
```

```bash
$ corepack enable
```

```bash
$ corepack enable pnpm
```
---

### Continue with : Create a Payload CMS Application [payload/publish-payload-app](../payload/publish-payload-app.md#create-a-payload-cms-application)

---

Note:
- In any of your computers there must NOT be a package.json file in your user's home directory. It can exist if you run `npm install` or `pnpm install` command while you are in your home directory. So before executing these commands first check your current directory.
- Existence of `package.json` file in your home directory can lead to unexpected errors.

-----
References:
- Node Version Manager: https://github.com/nvm-sh/nvm
- https://pnpm.io/docker
