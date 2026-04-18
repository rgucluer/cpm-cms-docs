## Create a Payload CMS Application

On Developer PC


```bash
cd <workspace-full-path>
```

```bash
pnpx create-payload-app@latest -t website
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
◇  Found latest version of Payload 3.81.0
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
│    - Getting Started: https://payloadcms.com/docs/getting-started/what-is-payload
│    - Configuration: https://payloadcms.com/docs/configuration/overview
│  
│  
│
└   Have feedback?  Visit us on GitHub: https://github.com/payloadcms/payload .
```

```bash
cd payload-app
```

```bash
pnpm approve-builds
```

```bash
corepack use pnpm@latest-10
```

```bash
pnpm dev
```

```bash
> payload-app@1.0.0 dev <workspace-full-path>/payload-app
> cross-env NODE_OPTIONS=--no-deprecation next dev

   ▲ Next.js 15.4.10
   - Local:        http://localhost:3000
   - Network:      <dev-pc-local-ip>:3000
   - Environments: .env

 ✓ Starting...
 Attention: Next.js now collects completely anonymous telemetry regarding usage.
This information is used to shape Next.js' roadmap and prioritize features.
You can learn more, including how to opt-out if you'd not like to participate in this anonymous program, by visiting the following URL:
https://nextjs.org/telemetry

 ✓ Ready in 1876ms
```

- Visit http://localhost:3000

- Visit http://localhost:3000/admin

- Create a new user (admin user) or Login to existing Admin user.

- Payload Dashboard opens.

- Click "Seed your database"

- Wait seeding to finish, then click visit your website link

- http://localhost:3000/ opens and homepage is rendered.

- Switch to terminal, <kbd>CTRL</kbd> + <kbd>C</kbd> to stop service

---

Continue with [Create a new private respository on GitHub](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)
