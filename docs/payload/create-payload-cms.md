## Create a Payload CMS Application

https://github.com/payloadcms/payload/tree/main/templates/website

On Developer PC

```bash
$ cd < workspace-full-path >
```

```bash
$ pnpx create-payload-app@latest -t website
```

```bash
! Corepack is about to download https://registry.npmjs.org/pnpm/-/pnpm-11.5.0.tgz
? Do you want to continue? [Y/n] 
```
<kbd>n</kbd> + <kbd>Enter</kbd>


```bash
? The next packages will now be built: @swc/core.
Do you approve? (y/N)
```

<kbd>y</kbd> + <kbd>Enter</kbd>

```bash
┌   create-payload-app 
│
◇  ────────────────────────────────────────────╮
│                                               │
│  Welcome to Payload. Let's create a project!  │
│                                               │
├───────────────────────────────────────────────╯
│
◆  Project name?
│  payload-app
└
```

```bash

◆  Select a database
│  ○ Cloudflare D1 SQlite
│  ● MongoDB
│  ○ PostgreSQL
│  ○ SQLite
│  ○ Vercel Postgres
└
◆  Enter MongoDB connection string
│  mongodb://127.0.0.1/payload-app
└
- Delete the current value. 
  - Copy Database URL from Mongo DB
    - Coolify UI -> Project -> cpm-cms -> Databases: mongodb-payload -> 
      Mongo URL (public) -> Reveal & Copy
  - Paste Database URL in terminal, DO NOT press Enter 
  - Change IP address with the IP address of the VM. Get the IP from multipass list command 
  - Press Enter to continue
│
◆  Select a coding agent to install the Payload skill for
│  ○ Claude Code
│  ○ Codex
│  ○ Cursor
│  ● None
└

I choosed None, choose as you wish, and continue

There is an error at this stage. Payload CMS uses pnpm version 10, but script may install pnpm version 11.

```bash
◇  Found latest version of Payload 3.85.0
│
◇  Using pnpm.
│  
│
■  Error installing dependencies: Command failed with exit code 1: pnpm install
│  [WARN] The "pnpm" field in package.json is no longer read by pnpm. The following keys were ignored: "pnpm.onlyBuiltDependencies". See https://pnpm.io/settings for the new home of each setting.
│  [ERR_PNPM_UNSUPPORTED_ENGINE] Unsupported environment (bad pnpm and/or Node.js version)
│  
│  Your pnpm version is incompatible with "< payload-app-full-path >".
│  
│  Expected version: ^9 || ^10
│  Got: 11.5.0
│  
│  This is happening because the package's manifest has an engines.pnpm field specified.
│  To fix this issue, install the required pnpm version globally.
│  
│  To install the latest version of pnpm, run "pnpm i -g pnpm".
│  To check your pnpm version, run "pnpm -v"..
■  Error installing dependencies
│
◇  Payload project successfully created!
│
◇   Next Steps 
│
│  
│  Launch Application:
│  
│    - cd ./payload-app
│    - pnpm dev or follow directions in README.md
│  
│  Documentation:
│  
│    - Getting Started
│    - Configuration
│  
│  
│
└   Have feedback?  Visit us on GitHub.
```

```bash
$ cd payload-app
```

```bash
$ corepack use pnpm@latest-10
```

```bash
Installing pnpm@10.34.1 in the project...

Lockfile is up to date, resolution step is skipped
Already up to date
Done in 1s using pnpm v10.34.1
```

---

Continue with [Create a new private respository on GitHub](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)
