# Publish a Payload CMS on a Virtual Machine

We will use the Payload Website template, https://github.com/payloadcms/payload/tree/main/templates/website .

https://payloadcms.com/docs/getting-started/what-is-payload

https://payloadcms.com/docs/production/deployment

## Open Coolify Web UI

Visit `https://coolify.devserver1.<domain-name>`

## Coolify - Add a new Project

- Coolify Web UI -> Projects -> Add
  - Name: cpm-cms
  - Description: Payload website template for Coolify
  - Continue

## Create a MongoDB using Coolify

- Coolify UI -> Projects -> cpm-cms (production) 
  - Resources +New
  - Databases
    - Mongo DB 
        - Configuration:
          - Name: mongodb-payload
          - Proxy
            - Public Port: 27017
          - Save
        - Resource Limits
          - Number of CPUs: 0.25
          - CPU Weight: 256
          - Limit Memory:
            - Soft Memory Limit: 1g
            - Maximum Memory Limit: 1g
            - Swappiness: 1
            - Maximum Swap Limit: 1g
          - Save
      - Start
        - Close "Database Startup" form after "Database started." message
        - Check for green Running (healthy) label
      - Coolify UI -> Projects -> cpm-cms (production) 
        - mongodb-payload
          - Configuration - General
            - Check : Proxy: Make it publicly available
              - Wait
                - We will use Mongo URL in later steps.
                - Copy Mongo URL (public) after revealing the string

## Create a local copy of Payload CMS Website template

```bash
corepack use pnpm@latest-10
```

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
Delete the current value. Paste here. Change IP address with the IP address of the VM. 
│
◇  Found latest version of Payload 3.69.0
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

```bash
cd payload-app
```

```bash
pnpm approve-builds
```

```bash
pnpm dev
```

```bash
> payload-app@1.0.0 dev <workspace-full-path>/payload-app
> cross-env NODE_OPTIONS=--no-deprecation next dev

 ⚠ Warning: Found multiple lockfiles. Selecting /home/john/workspace_linux/pnpm-lock.yaml.
   Consider removing the lockfiles at:
   * /home/john/workspace_linux/payload-app/pnpm-lock.yaml

   ▲ Next.js 15.4.10
   - Local:        http://localhost:3000
   - Network:      http://192.168.1.21:3000
   - Environments: .env

 ✓ Starting...
 ✓ Ready in 1876ms
```

Visit http://localhost:3000

Visit http://localhost:3000/admin

Create a new user (admin user).

Payload Dashboard opens.

Switch to terminal, <kbd>CTRL</kbd> + <kbd>C</kbd> to stop service

---

## [Create a new private respository on GitHub](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)

Private repository name: cpm-cms

## Add a new remote for cpm-cms

Set a GitHub repository as the remote for cpm-cms

https://docs.github.com/en/get-started/git-basics/managing-remote-repositories

```bash
cd <workspace-full-path>/payload-app
```

```bash
git remote add origin git@github.com:<github-username>/cpm-cms.git
```

## git push the Payload CMS code to GitHub repository

```bash
cd <workspace-full-path>/payload-app
```

```bash
git status
```

Edit <workspace-full-path>/payload-app/.gitignore file . Add "*.db" at the end of file . Save & exit .
```text
.....
/playwright/.cache/

*.db
```

<workspace-full-path>/payload-app/docker-compose.yml

```yaml
name: payload
networks:
  coolify:
    name: coolify
    external: true
services:
  payload:
    restart: unless-stopped
    extra_hosts:
      - 'host.docker.internal:host-gateway'
    build:
      context: .
      dockerfile: Dockerfile
    environment:
      # Limits Node.js to 2GB of RAM to prevent the container from crashing the VPS
      - 'NODE_OPTIONS=--max-old-space-size=2048'
    healthcheck:
      test: 'curl -f --head http://127.0.0.1:3000 || exit 1'
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 15s
    expose:
      - 3000    
    volumes:
      - node_app:/app
    working_dir: /app/
    networks:
      - coolify
    labels:
      traefik.enable: true
      coolify.managed: true
      coolify.proxy: true

volumes:
  node_app:

```

- Coolify UI -> Servers -> devserver1 -> Proxy -> Dynamic Configurations
  - payload-app.yaml
  ```yaml
  http:
    middlewares:
      root-to-www:
        redirectRegex:
          regex: '^((https?:\/\/)|).devserver1.my-domain.com'
          replacement: 'https://www.devserver1.my-domain.com'
          permanent: true
    routers:
      redirect-root:
        rule: 'Host(`my-domain.com`)'
        entryPoints:
          - http
          - https
        middlewares:
          - root-to-www
        tls:
          certResolver: letsencrypt
          domains:
            main: 'devserver1.my-domain.com'
        service: noop@internal
      payload-https:
        rule: 'Host(`www.my-domain.com`)'
        entryPoints:
          - https
        tls:
          certResolver: letsencrypt
          domains:
            main: 'devserver1.my-domain.com'
            sans:
              - '*.devserver1.my-domain.com'
        service: payload@docker
  ```

Set payload-https service value from actual service name in Traefik services

- Coolify UI -> Projects -> payload-vps -> Applications: payload -> 
  Configuration -> Advanced -> Container Names
  - Custom Container Name: payload
  - Check service name in Traefik Dashboard

<workspace-full-path>/payload-app/Dockerfile

```docker
# syntax=docker.io/docker/dockerfile:1

# To use this Dockerfile, you have to set `output: 'standalone'` in your next.config.js file.
# From https://github.com/vercel/next.js/blob/canary/examples/with-docker/Dockerfile

FROM node:22.17.0-alpine AS base
# Check https://github.com/nodejs/docker-node/tree/b4117f9333da4138b03a546ec926ef50a31506c3#nodealpine to understand why libc6-compat might be needed.
RUN apk add --no-cache libc6-compat python3 make g++ git curl
RUN corepack enable pnpm

# ========================================
# Dependencies Stage
# ========================================

# Install dependencies only when needed
FROM base AS deps
WORKDIR /app

# Install dependencies based on the preferred package manager
COPY package.json yarn.lock* package-lock.json* pnpm-lock.yaml* ./
RUN \
  if [ -f yarn.lock ]; then yarn --frozen-lockfile; \
  elif [ -f package-lock.json ]; then npm ci; \
  elif [ -f pnpm-lock.yaml ]; then pnpm i --frozen-lockfile; \
  else echo "Lockfile not found." && exit 1; \
  fi

# ========================================
# Builder Stage
# ========================================
FROM base AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .

# Next.js collects completely anonymous telemetry data about general usage.
# Learn more here: https://nextjs.org/telemetry
# Uncomment the following line in case you want to disable telemetry during the build.
ENV NEXT_TELEMETRY_DISABLED 1
ENV NEXT_PRIVATE_STANDALONE=true

RUN \
  if [ -f yarn.lock ]; then yarn run build; \
  elif [ -f package-lock.json ]; then npm run build; \
  elif [ -f pnpm-lock.yaml ]; then pnpm run build; \
  else echo "Lockfile not found." && exit 1; \
  fi

# ========================================
# Runner Stage
# ========================================
FROM base AS runner
WORKDIR /app

ENV NODE_ENV production

# Uncomment the following line in case you want to disable telemetry during runtime.
ENV NEXT_TELEMETRY_DISABLED 1

RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

# Remove this line if you do not have this folder
COPY --from=builder --chown=nextjs:nodejs /app/public ./public

# Set the correct permission for prerender cache
RUN mkdir .next
RUN chown nextjs:nodejs .next

# Automatically leverage output traces to reduce image size
# https://nextjs.org/docs/advanced-features/output-file-tracing
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static

USER nextjs

EXPOSE 3000

# server.js is created by next build from the standalone output
# https://nextjs.org/docs/pages/api-reference/next-config-js/output
CMD node server.js

```

TODO: Add any file creation/change to original code. Or make the modified source repository public.
.dockerignore
next.config.js : 
  - pathname for media
  - output: 'standalone'
  - Dockerfile : media directory
  - docker-compose.yml : 
    - container_name: payload
    - labels
  - .env.example


```bash
git add .
```

```bash
git commit
```

Enter commit message. Save & exit.

```bash
git push origin main
```

## Create a new branch "dev" on Github

https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository

## Git push to dev branch

After we make changes to source code, and commit the changes we can push the code to the original repo on GitHub.

https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-branches

```bash
git checkout -b dev
```

```bash
git push origin dev
```

```bash
pnpm build
```

## Run Virtual Machine
```bash
multipass start coolvm
```

## Coolify - Add a new Source

- Coolify Web UI -> Sources -> Add+
  - New GitHub App
    - Name: cpm-cms
    - Continue
    - Automated Installation
      - Webhook Endpoint
        - Select from select box
          - Select  https://coolify.devserver1.<my-domain.com>
        - Register Now
        - Confirm access ( GitHub Authentication )        
          - Authenticate
        - Create App name
          - GitHub App Name : cpm-cms
          - Click "Create GitHub App for <my-github-username>"
    - Click Install Repositories on GitHub
    - Authorization Request for GitHub - Install cpm-cms
      - Click: Only select repositories
        - Select from "Select repositories"
          - <my-github-username>/cpm-cms
      - Click Install
  - We are back in Sources -> <my-github-app>
  - Save

## Add Source to the Project
- Coolify Web UI -> Projects -> cpm-cms -> production
  - Click Resources +New
    - Applications -> Git Based
      - Click Private Repo. with GitHub App
    - Select a GitHub App: Click cpm-cms button
      - Repository: cpm-cms
      - Click Load Repository
        - Branch: dev
        - Build Pack: Docker Compose
        - Change Docker Compose Location to:
          - /docker-compose.yml
          - ATTENTION : File extension is .yml not .yaml. Please check & edit.
        - Continue
        - If successful, this loads docker-compose.yml, opens configuration menu.

## Configure the Project
- Coolify Web UI -> Projects -> cpm-cms ( production )
  - Applications -> payload ( Name application as payload )
    - Configuration -> General
      - Name: payload
      - Description: Payload template (website) for Coolify
      - Build Pack: Docker Compose
      - Domains -> Domains for payload:
        - https://devserver1.my-domain.com,https://www.devserver1.my-domain.com
      - Build
        - Check: Preserve Repository During Deployment
      - Docker Compose Location : /docker-compose.yml
      - Docker Compose
        - Check : Escape special characters in labels
      - Save

    - Configuration -> Advanced
      - Auto Deploy: Uncheck
      - Inject Build Args to Dockerfile: Check
      - Include Source Commit in Build: Uncheck
      - Force Https: Check
      - Strip Prefixes: Check
      - Container Names:
        - Consistent Container Names: Uncheck
        - Custom Container Name: payload
          - Save
      - Network - Connect To Predefined Network : Check

    - Configuration -> Environment Variables
      - Check: Use Docker Build Secrets
      - Production Environment Variables
        - Add the following variables
      - APP_ROOT_DN: devserver1.my-domain.com
        - Check: Available at Buildtime , Check: Available at Runtime
        - Save
      - DATABASE_URL: Get value from mongodb-payload Mongo URL (public), and paste here. Change IP address to VM IP address
        - Check: Available at Buildtime , Check: Available at Runtime
        - Save, Close Form, Update
      - HOSTNAME: 0.0.0.0
        - Check: Available at Buildtime , Check: Available at Runtime
        - Save, Close Form, Update
      - NEXT_PUBLIC_SERVER_URL : https://www.devserver1.<domain-name>
        - Check: Available at Buildtime , Check: Available at Runtime
        - Save, Close form, Update
      - NODE_OPTIONS: --no-deprecation --max-old-space-size=3072
        - Check: Available at Buildtime , Check: Available at Runtime
        - Save, Close form, Update
      - PAYLOAD_SECRET: <copy value from local copy .env file >
        - Check: Available at Buildtime , Check: Available at Runtime
        - Save, Close form, Update
    - Configuration -> Git Source
      - Repository:
        - <github-username>/cpm-cms
      - Branch: 
        - dev
        - Save, Close form

  - Coolify Web UI -> Projects -> cpm-cms ( production )
    - Resource Limits
      - Limit CPUs ( https://docs.docker.com/engine/containers/run/#cpu-share-constraint )
        - Number of CPUs: 0.5
          - Number is a fractional number. 0.000 means no limit.
        - CPU sets to use: Empty
        - CPU Weight: 512
        - Soft Memory Limit: 2g
        - Maximum Memory Limit: 2g
        - Swappiness: 1
        - Maximum Swap Limit: 2g
        - Save
        - ( Set Memory values to according to your setup. )

  - Coolify UI -> Servers -> server1 -> Proxy -> Dynamic Configurations
    - payload-app.yaml
    ```yaml
    http:
      middlewares:
        root-to-www:
          redirectRegex:
            regex: '^((https?:\/\/)|).my-domain.com'
            replacement: 'https://www.my-domain.com'
            permanent: true
      routers:
        redirect-root:
          rule: Host(`my-domain.com`)
          entryPoints:
            - http
            - https
          middlewares:
            - root-to-www
          tls:
            certResolver: letsencrypt
            domains:
              main: my-domain.com
          service: noop@internal
        payload-https:
          rule: Host(`www.my-domain.com`)
          entryPoints:
            - https
          tls:
            certResolver: letsencrypt
            domains:
              main: my-domain.com
              sans:
                - '*.my-domain.com'
          service: payload@docker
    ```
    - Save

  - Coolify UI -> Projects -> cpm-cms (production) -> cpm-cms
    - Deploy/Redeploy
    - Deploy takes time.
      - Click Show Debug Logs
      - Watch for "New container started." message.
      - Wait for a couple of minutes, watch Running label. Check it stays "Running (healthy)".

  - Coolify UI -> <server-name> -> Restart Proxy
    - Confirm ... -> Restart Proxy

## Get Payload service name
- Open Traefik dashboard -> HTTP Services 
  Note/Copy the name starting with payload-...

## Change service name in Traefik dynamic configuration
- Coolify UI -> Servers -> server1 -> Proxy -> Dynamic Configurations
  - Edit payload-app.yaml
  - http -> routers -> payload-https:
    - Paste/enter the payload service name we copied.
    - Save

## Check the Application
- Check `https://www.devserver1.<domain-name>`
  - Works, good `:)` .

- Login ( https://www.devserver1.<domain-name>/admin )
  - Click "Seed your database", wait ...

- Visit `https://www.devserver1.<domain-name>`

- Learn more ... [https://payloadcms.com/docs/getting-started/what-is-payload](https://payloadcms.com/docs/getting-started/what-is-payload)


## Continue with
- [Production Environment](docs/production.md)


## References
- https://github.com/coollabsio/coolify-examples/tree/v4.x
- https://github.com/coollabsio/coolify-examples/tree/v4.x/nextjs/ssr 
- https://github.com/coollabsio/coolify-examples/tree/v4.x/nextjs/spa
- https://coolify.io/self-hosted/
- https://coolify.io/docs/knowledge-base/proxy/traefik/redirects
- https://coolify.io/docs/knowledge-base/docker/compose
