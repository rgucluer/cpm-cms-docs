# Publish a Payload CMS on a Virtual Machine

We will use the Payload Website template, https://github.com/payloadcms/payload/tree/main/templates/website .

## Coolify - Add a new Project

- Coolify Web UI -> Projects -> Add
  - Name: cpm-cms
  - Description: Payload website template for Coolify
  - Continue

### Create a Mongo DB service on Coolify
- Coolify Web UI -> Projects -> cpm-cms (production) -> Resources -> New
  - Databases -> MongoDB
  - Start
    - "Database started."
      - Close Database  Startup form
  - Configuration -> General
    - Name: mongodb-payload
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

We will use the dev branch of the git repository

## [Create Payload CMS from a template](unsuccessful-payload-cms-vm.md/create-payload-cms.md)

## [Create a new private respository on GitHub](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)

## Add a new remote for cpm-cms

Set newly created GitHub repository as the remote for cpm-cms

https://docs.github.com/en/get-started/git-basics/managing-remote-repositories

```bash
cd <workspace-full-path>/cpm-cms
```

```bash
git remote add origin <github-repo-url>
```

## git push the Payload CMS code to GitHub repository

```bash
cd <workspace-full-path>/cpm-cms
```

```bash
git status
```
```bash
On branch main
nothing to commit, working tree clean
```

```bash
git push origin main
```

## Create a new branch "dev" on Github

https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository

## Git push to dev branch

After we make changes to source code, and commit the changes we can push the code to the original repo on GitHub.



```bash
git push origin dev
```

## Coolify - Add a new Source

- Coolify Web UI -> Sources -> Add+
  - New GitHub App
    - Name: cpm-cms-app3
    - Continue
    - Automated Installation
      - Webhook Endpoint
        - Select from select box
          - Select  https://coolify.devserver1.<my-domain.com>
        - Register Now
        - Confirm access ( GitHub Authentication )        
          - Authenticate
        - Create App name
          - GitHub App Name : cpm-cms-app3
          - Click "Create GitHub App for <my-github-username>"
    - Click Install Repositories on GitHub
    - Authorization Request for GitHub - Install cpm-cms-app
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
    - Select a GitHub App: Click cpm-cms-app3 button
      - Repository: cpm-cms
      - Click Load Repository
        - Branch: dev
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
        - https://www.devserver1.my-domain.com
      - Save
    - Configuration -> Environment Variables
      - Production Environment Variables
      - NEXT_PUBLIC_SERVER_URL : https://www.devserver1.my-domain.com
        - Update
      - NODE_OPTIONS: --no-deprecation --max-old-space-size=8192
        - Update
    - Configuration -> Git Source
      - Repository:
        - <github-username>/cpm-cms
      - Branch: 
        - dev
        - Save

### On Dev PC: Modify <payload-app-full-path>/package.json

```js
.....
  "scripts": {
    "build": “pnpm next build --experimental-build-mode compile",
.....
```

## pnpm install

On Dev PC

```bash
cd <app-full-path>
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
git push origin dev
```

## Edit .env file on Virtual Machine

### Copy Content of Mongo URL (public)
- Coolify Web UI -> Projects -> cpm-cms (production)
  - Databases -> mongodb-database-...
    - Configuration -> General -> Network
      - Copy value of Mongo URL (public)

- Coolify Web UI -> Projects -> cpm-cms (production) -> payload 
  - Environment Variables
    - Add 
      - DATABASE_URL
        - < Paste copied value here >
        - UnCheck : Available at Buildtime
        - Checked : Available at Runtime
      - Save
      - Close Form
    - Add 
      - HOST
        - 0.0.0.0
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
      - Soft Memory Limit: 2g
      - Maximum Memory Limit: 2g
        - Memory limit (format: <number>[<unit>]). Number is a positive integer. Unit can be one of b, k, m, or g. 
      - Swappiness: 1
      - Maximum Swap Limit: 2g
        - https://docs.docker.com/reference/compose-file/services/#memswap_limit
      - Save

Set values suitable for your setup.

## Create a dynamic configuration file for Payload CMS
- Coolify Web UI -> Servers -> devserver1 -> Proxy -> Dynamic Configurations
  - Add
    - payload.yaml

```yaml
http:
  routers:
    payload-https:
      entryPoints:
        - https
      service: payload
      rule: Host(`www.devserver1.my-domain.com`)
      tls:
        certresolver: letsencrypt
    payload-http:
      middlewares:
        - redirect-to-https
      entryPoints:
        - http
      service: payload
      rule: Host(`www.devserver1.my-domain.com`)
      
  services:
    payload:
      loadBalancer:
        servers:
          - 'url=https://www.devserver1.my-domain.com'

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
    - Deployment Log:x
      - Watch progress
        - After "Starting new application" message:
          - Click - "Show Debug Logs
            - If no errors occur, A green "Running" label occurs on top menu.


## Troubleshooting

MongoParseError: Invalid scheme, expected connection string to start with "mongodb://" or "mongodb+srv://"

### failed to solve: process "/bin/sh -c pnpm run build" did not complete successfully: exit code: 1
