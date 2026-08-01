# Publish a Payload CMS on a Virtual Machine

## Open Coolify Web UI of Virtual Machine

- Start Virtual Machine
- Visit `https://coolify.< dev-domain-name >`

## Coolify - Add a new Project

- Coolify Web UI -> Projects -> Add
  - New Project
    - Name: cpm-cms
    - Description: Payload website template for Coolify
    - Continue

## Create a MongoDB using Coolify [payload/create-mongodb](create-mongodb.md)

## Install nvm, and Node on Development PC [nextjs/install-nvm-node](../nextjs/install-nvm-node.md)

## Create a Payload CMS Application

### Option One: Create app with pnpx [payload/method-one/create-payload-cms-m1](method-one/create-payload-cms-m1.md)
If you are creating a new Payload CMS application, and did not committed to a git repository yet, use this method.

### Option Two: Create app with cloning a git repository [payload/method-two/create-payload-cms-m2](method-two/create-payload-cms-m2.md)
If you already have a git repository, use this method.

## Add a new source ( GitHub App ) to Coolify [coolify/add-new-source](../coolify/add-new-source.md)

## Add Source to the Project [coolify/add-source-to-project](../coolify/add-source-to-project.md)
We added the source (GitHub App) to Coolify Sources section in previous step. Now we will connect that to our Coolify Project . 

## Configure the Payload Project [coolify/configure-payload-dev](../coolify/configure-payload-dev.md)

## Deploy / Redeploy Payload Application
- !ATTENTION! Deploy operation will delete all content. 
  - TODO: Implement Backup/Restore operations before/after deployment.
- Coolify UI on VM (https://coolify.< dev-domain-name >)
  - Projects : < coolify-project-name > -> Applications : < coolify-application-name >
    - Stop service if it is already running. 
    - Reload Compose File
    - Save
    - Advanced -> Force deploy (without cache)
  - Wait until the message "Container payload-... Started"
    - Click debug icon for more build information
  - Check the green label above, if it stays green "Running(healthy) for a couple of ten seconds...
  - After deployment, node will build Payload CMS. It takes time (3 - 4 minutes).

## Set Traefik for the new application [coolify/set-traefik-for-new-app](../coolify/set-traefik-for-new-app.md)

## Check the Application
- Check `https://www.< dev-domain-name >`
  - Works, good `:)` .
  - Not working, bad `:(`, try [troubleshooting](#troubleshooting).

- Click `Visit the admin dashboard`
  - `https://www.< dev-domain-name >/admin`
  - Create your first user, or login
  - Click "Seed your database", wait ...
    - If successful we get the `Database seeded! You can now visit your website` message.
      - Or just a "done" prompt right of "Seed your database" link.
    - If process fails "An error occured while seeding", check for file permissions and ownership in payload service container. For more information read troubleshooting below.

- Visit `https://www.< dev-domain-name >` , or click "visit your website" link

- Page renders with images.

- Learn more ... [https://payloadcms.com/docs/getting-started/what-is-payload](https://payloadcms.com/docs/getting-started/what-is-payload)

## Continue with: 

### Production Environment [production](../../README.md#production-environment-production)

### Daily Development Operations [daily-dev.md](../development.md#daily-operations-of-development-environment-daily-dev)

---

## References
- https://payloadcms.com/docs/production/deployment
- https://github.com/coollabsio/coolify-examples/tree/v4.x/nextjs/ssr 
- https://github.com/coollabsio/coolify-examples/tree/v4.x/nextjs/spa
- https://coolify.io/self-hosted/
- https://coolify.io/docs/knowledge-base/proxy/traefik/redirects
- https://coolify.io/docs/knowledge-base/docker/compose
- https://nextjs.org/docs/messages/next-image-unconfigured-localpatterns


## TODO: 
- Add backup/restore documentation
- Implement Payload CMS as a Coolify Service

---

## Troubleshooting

### 404 page not found

[404 page not found](../troubleshoot/t404-page-not-found.md)

---

### Error: #1 [internal] load local bake definitions

```bash
$ corepack enable pnpm && pnpm install --frozen-lockfile;

 Error: #1 [internal] load local bake definitions

```
- Apply [troubleshoot/frozen-lockfile](../troubleshoot/frozen-lockfile.md) 

### Payload does not render images
- In progress
- Apply [troubleshoot/render-images](../troubleshoot/render-images.md) 

### Deployment Error [ERR_PNPM_IGNORED_BUILDS]

During VM or VPS Payload CMS deployment

```bash
[ERR_PNPM_IGNORED_BUILDS] Ignored build scripts: esbuild@0.28.1, sharp@0.34.2, sharp@0.34.5, unrs-resolver@1.12.2

Run "pnpm approve-builds" to pick which dependencies should be allowed to run scripts.
```

Run `pnpm approve-builds` command on development machine locally, it will create a pnpm-workspace.yaml file. Add this file name to Dockerfile initial copy command

```Dockerfile
# .....
# Install dependencies based on the preferred package manager
COPY package.json yarn.lock* package-lock.json* pnpm-lock.yaml* pnpm-workspace.yaml ./
# .....
```

Apply steps in [troubleshoot/frozen-lockfile](../troubleshoot/frozen-lockfile.md)

### Error during seeding
- An error occured while seeding
  - Check for proper file/directory ownership in payload container
  - Still getting the error, Apply [troubleshoot/seeding](../troubleshoot/seeding.md)

### Container keeps restarting
`Error: Cannot find module '/home/node/app/server.js'`
- Comment out existing volumes in docker-compose.yml 
- Do not use volumes in payload service

### Payload build error
- ERR_PNPM_OUTDATED_LOCKFILE  Cannot install with "frozen-lockfile" because pnpm-lock.yaml is not up to date with < ROOT >/package.json
- Apply [troubleshoot/frozen-lockfile](../troubleshoot/frozen-lockfile.md)

### Error: It looks like you're trying to use `tailwindcss` directly as a PostCSS plugin
- Apply [troubleshoot/tailwindcss](../troubleshoot/tailwindcss.md)

### SSL Certificate Error net::ERR_CERT_AUTHORITY_INVALID
- Check DNS settings on your InternetServiceProvider
- Check Firewall settings
- Check [Server Traefik configuration](../coolify/configure-coolify-traefik.md)
- Check [Application Traefik configuration](../coolify/set-traefik-for-new-app.md) for details
- Make necessary changes, restart Proxy, Redeploy application

### pnpm dev ends in infinite loop, FATAL: An unexpected Turbopack error occurred.

Log contains "Next.js package not found"

- Apply [troubleshoot/frozen-lockfile](../troubleshoot/frozen-lockfile.md) for details

### Updates were rejected because the tip of your current branch is behind
  - Read Git Documentation https://git-scm.com/docs/git-pull 

---

### References:
- We use the Payload Website template, https://github.com/payloadcms/payload/tree/main/templates/website .
- https://payloadcms.com/docs/getting-started/what-is-payload
- https://payloadcms.com/docs/production/deployment

