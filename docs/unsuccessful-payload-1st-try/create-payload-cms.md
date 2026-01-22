## [Create Payload CMS from a template](mongo-db/create-payload-cms.md)

https://github.com/payloadcms/payload/tree/main/templates/website

- Add your ssh keys to ssh-agent

```bash
cd /home/<local-user-name>/<local-workspace>
```

```bash
pnpx create-payload-app cpm-cms -t website
```

```bash
┌   create-payload-app 
│
◇  ────────────────────────────────────────────╮
│                                               │
│  Welcome to Payload. Let's create a project!  │
│                                               │
├───────────────────────────────────────────────╯
│
◆  Select a database
│  ○ Cloudflare D1 SQlite
│  ● MongoDB
│  ○ PostgreSQL
│  ○ SQLite
│  ○ Vercel Postgres
```

```bash
 Enter MongoDB connection string
│  mongodb://127.0.0.1/cpm-cms
◇  Found latest version of Payload 3.64.0
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
│    - cd ./cpm-cms
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
