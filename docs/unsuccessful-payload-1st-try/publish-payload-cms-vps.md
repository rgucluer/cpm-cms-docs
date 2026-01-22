# Publish a Payload CMS on a Virtual Private Server

We will use the Payload Website template, https://github.com/payloadcms/payload/tree/main/templates/website .

## Coolify - Add a new Project

- Coolify Web UI -> Projects -> Add
  - Name: payload-vps
  - Description: Payload website template for Coolify
  - Continue

### Create a Mongo DB service on Coolify
- Coolify Web UI -> Projects -> payload-vps (production) -> Resources -> New
  - Databases -> MongoDB
  - Start
    - "Database started."
      - Close Database  Startup form
  - Configuration -> General
    - Name: mongodb-database-vps
    - Save
  - Proxy
    - Public Port: 27017
    - Check: Make it publicly available
  - Copy Content of Mongo URL (public)

### Mongo DB Resource Limits
- Limits CPUs: 
  - Number of CPUs: 0.5
  - CPU Weight: 256
- Limit Memory
  - Soft Mem. Lim. : 1g
  - Maximum Memory Limit: 1g
  - Swappiness: 1
  - Maximum Swap Limit: 1g
- Save

We will use the main branch of the git repository

## Coolify - Add a new Source

- Coolify Web UI -> Sources -> Add+
  - New GitHub App
    - Name: cpm-cms-app-main
    - Continue
    - Automated Installation
      - Webhook Endpoint
        - Select from select box
          - Select  https://coolify.<my-domain.com>
        - Register Now
        - Confirm access ( GitHub Authentication )        
          - Authenticate
        - Create App name
          - GitHub App Name : cpm-cms-app-main
          - Click "Create GitHub App for <my-github-username>"
    - Click Install Repositories on GitHub
    - Authorization Request for GitHub - Install cpm-cms-app-main
      - Click: Only select repositories
        - Select from "Select repositories"
          - <my-github-username>/cpm-cms
      - Click Install
  - We are back in Sources -> <my-github-app>
  - Save

## Add Source to the Project
- Coolify Web UI -> Projects -> cpm-cms -> production
  - Click +New
    - Applications -> Git Based
      - Click Private Repo. with GitHub App
    - Select a GitHub App: Click cpm-cms-app-main button
      - Repository: cpm-cms
      - Click Load Repository
        - Branch: main
        - Build Pack: Docker Compose
        - Change Docker Compose Location to:
          - /prod-docker-compose.yaml
        - Continue
        - If successful, this loads prod-docker-compose.yaml, and adds the Domains Option.

- Coolify Web UI -> Projects -> cpm-cms -> production 
  - Applications -> payload ( Name application as payload )
    - Configuration -> General
      - Name: payload
      - Build
        - Check: Preserve Repository During Deployment
      - Domains -> Domains for payload:
        - https://www.my-domain.com
      - Save
    - Configuration -> Environment Variables
      - NEXT_PUBLIC_SERVER_URL : https://www.my-domain.com
        - Update
      - NODE_OPTIONS: --no-deprecation --max-old-space-size=2048
        - Update
    - Configuration -> Git Source
      - Repository:
        - <github-username>/cpm-cms
      - Branch: 
        - main
        - Save

### On Dev PC: Check <payload-app-full-path>/package.json

```js
.....
  "scripts": {
    "build": “pnpm next build --experimental-build-mode compile",
.....
```

## pnpm install

On Dev PC

```bash
cd /data/coolify/applications/<generated-app-directory-name>
```

```bash
pnpm install
```

```bash
pnpm run dev
```

Check http://localhost:3000
Check http://localhost:3000/admin

If it is OK, continue ...

`CTRL` + `C` stop server

```bash
git status
```

```bash
git add .
```

```bash
git commit
```

```bash
git push origin main
```

On VPS change directory to /data/

## Edit .env file on Virtual Private Server

### Copy Content of Mongo URL (public)
- Coolify Web UI -> Projects -> cpm-cms -> production 
  - Databases -> mongodb-database-...
    - Configuration -> General -> Network
      - Copy value of Mongo URL (public)

- Coolify Web UI -> Projects -> cpm-cms (production) -> payload 
  - Environment Variables
    - Add 
      - DATABASE_URI : < Paste copied value here >
        - UnCheck : Available at Buildtime
        - Checked : Available at Runtime
      - Save
      - Close Form
    - Add 
      - HOST
        - <vps-ip-address>
        - UnCheck : Available at Buildtime
        - Checked : Available at Runtime
      - Save
      - Close Form

## Set Project Resource Limits

- Coolify Web UI -> Projects -> cpm-cms -> production -> Applications -> payload
  - Resource Limits
    - Limit CPUs ( https://docs.docker.com/engine/containers/run/#cpu-share-constraint )
      - Number of CPUs: 0.5
        - Number is a fractional number. 0.000 means no limit.
      - CPU sets to use: empty
      - CPU Weight: 256
      - Soft Memory Limit: 3g
      - Maximum Memory Limit: 3g
        - Memory limit (format: <number>[<unit>]). Number is a positive integer. Unit can be one of b, k, m, or g. 
      - Swappiness: 1
      - Maximum Swap Limit: 3g
        - https://docs.docker.com/reference/compose-file/services/#memswap_limit
      - Save

Set values suitable for your setup.

## Create a dynamic configuration file for Payload CMS

- Coolify Web UI -> Servers -> server1 -> Proxy -> Dynamic Configurations
  - Add
    - Filename: payload.yaml

  ```yaml
  http:
  routers:
    payload-https:
      entryPoints:
        - https
      service: payload
      rule: Host(`www.my-domain.com`)
      tls:
        certresolver: letsencrypt
    payload-http:
      middlewares:
        - redirect-to-https
      entryPoints:
        - http
      service: payload
      rule: Host(`www.my-domain.com`)
  services:
    payload:
      loadBalancer:
        servers:
          - 'url=https://www.my-domain.com'
  ```
  
- Save & Close Configuration form

## Check Connect to Predefined Network
- Coolify Web UI -> Projects -> cpm-cms -> production -> Payload -> Advanced
  - Network
    - Check: Connect To Predefined Network


## Deploy the Application
- Coolify Web UI -> Projects -> cpm-cms -> production
  - Applications -> payload
  - Deploy or Redeploy
    - Deployment Log:
      - Watch progress
        - After "Starting new application" message:
          - Click - "Show Debug Logs
            - If no errors occur, A green "Running" label occurs on top menu.

