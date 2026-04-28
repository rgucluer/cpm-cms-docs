## Create a Payload CMS Application

On Developer PC


```bash
$ cd < workspace-full-path >
```

```bash
$ pnpx create-payload-app@latest -t website
```

```bash
Packages: +93
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Downloading @swc/core-linux-x64-musl@1.15.3: 14.64 MB/14.64 MB, done
Progress: resolved 101, reused 83, downloaded 10, added 93, done
╭ Warning ───────────────────────────────────────────────────────────────────────────────────╮
│                                                                                            │
│   Ignored build scripts: @swc/core@1.15.3.                                                 │
│   Run "pnpm approve-builds" to pick which dependencies should be allowed to run scripts.   │
│                                                                                            │
╰────────────────────────────────────────────────────────────────────────────────────────────╯
Downloading @swc/core-linux-x64-gnu@1.15.3: 12.40 MB/12.40 MB, done

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
  - Press Enter at the end of line to continue
│
◆  Select a coding agent to install the Payload skill for
│  ● Claude Code
│  ○ Codex
│  ○ Cursor
│  ○ None
└

I choosed None, choose as you wish, and continue

│
◇  Found latest version of Payload 3.84.1
│
◇  Using pnpm.
│  
◇  Successfully installed Payload and dependencies
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
$ pnpm approve-builds
```

```bash
$ corepack use pnpm@latest-10
```

```bash
Installing pnpm@10.33.2 in the project...

Lockfile is up to date, resolution step is skipped
Already up to date
Done in 1s using pnpm v10.33.2
```

```bash
$ pnpm dev
```

```bash
> payload-app@1.0.0 dev < workspace-full-path >/payload-app
> cross-env NODE_OPTIONS=--no-deprecation next dev

▲ Next.js 16.2.3 (Turbopack)
- Local:        http://localhost:3000
- Network:      < dev-pc-local-ip >:3000
- Environments: .env
✓ Ready in 374ms
- Experiments (use with caution):
  ⨯ turbopackServerFastRefresh

```

- Visit http://localhost:3000

- Visit http://localhost:3000/admin

- Create a new user (admin user) or Login to existing Admin user.

- Payload Dashboard opens.

- Click "Seed your database"

- Wait seeding to finish on terminal, then click visit your website link on browser

- http://localhost:3000/ opens and homepage is rendered with images.

- Switch to terminal, <kbd>CTRL</kbd> + <kbd>C</kbd> to stop service

---

Continue with [Create a new private respository on GitHub](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)
