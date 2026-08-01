# Create Payload CMS app with pnpx - On Developer PC

```bash
$ cd < workspace-full-path >
```

```bash
$ pnpx create-payload-app@3.86.0 -t website
```

```bash
Packages: +91
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Progress: resolved 100, reused 91, downloaded 0, added 0, done
(node:24731) MaxListenersExceededWarning: ...
? Choose which packages to build (Press <space> to select, <a> to toggle all, <i> to invert selection)
❯○ @swc/core
```

Press <kbd>Space</kbd> then <kbd>ENTER</kbd>

```bash
? The next packages will now be built: @swc/core.
Do you approve? (y/N)
```
<kbd>y</kbd> , <kbd>ENTER</kbd> to continue


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
Enter project name, and ENTER to continue

```bash
◆  Select a database
│  ○ Cloudflare D1 SQlite
│  ● MongoDB
│  ○ PostgreSQL
│  ○ SQLite
│  ○ Vercel Postgres
└
```

Select your database (up,down), ENTER to continue

```bash
◆  Enter MongoDB connection string
│  mongodb://127.0.0.1/payload-app
└
```
- Delete the current value. 
  - Copy Database URL from Mongo DB
    - Coolify UI -> Project -> payload-app -> Databases: mongodb-payload -> 
      Mongo URL (public) -> Reveal & Copy
  - Paste Database URL in terminal, DO NOT press Enter 
  - Change IP address with the IP address of the VM. Get the VM IP from multipass list command 
  - Press Enter to continue

```bash
◆  Select a coding agent to install the Payload skill for
│  ○ Claude Code
│  ○ Codex
│  ○ Cursor
│  ● None
└
```
I choosed None, choose as you wish, and continue

```bash
◇  Found latest version of Payload 3.86.0
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
└   Have feedback?  Visit us on GitHub: https://github.com/payloadcms/payload.
```

---

### Continue with : Make some changes in source code [payload/method-one/change-source-code](create-payload-cms-m1.md#make-some-changes-in-source-code-payloadmethod-onechange-source-code)

---

### References
- https://github.com/payloadcms/payload
- https://payloadcms.com/docs/getting-started/installation
