# Publish Payload CMS on a Virtual Private Server

## Open Coolify Web UI of Virtual Private Server

- Visit `https://coolify.< domain-name >`

## Coolify - Add a new Project

- Coolify UI -> Projects -> Add
  - New Project
    - Name: payload-project
    - Description: Payload template (website) for Coolify
    - Continue

## Create a MongoDB using Coolify [payload/create-mongodb-prod](create-mongodb-prod.md)

## Create a local copy of Payload CMS Website template

We created the files during development stage. Merge development to main branch. Combine with squash merge.

https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging

https://git-scm.com/cheat-sheet

On Developer PC

Commit all changes.

Merge dev branch to main branch

```bash
cd < workspace-full-path >/payload-app
```

```bash
git branch
```

```bash
git checkout main
```

```bash
git merge --squash dev
```

Fix any conflicts.

Commit and push your changes to Git repo

```bash
git commit
```

```bash
git push origin main
```

Delete already merged dev branch
```bash
git branch -d dev
```

Delete dev branch on remote
```bash
git push origin --delete dev
```

## Coolify - Add a new Source ( Virtual Private Server ) [coolify/add-new-source-vps](../coolify/add-new-source-vps.md)

## Add Source to the Project (VPS) [coolify/add-source-to-project-vps](../coolify/add-source-to-project-vps.md)

## Configure the Payload Project (VPS) [coolify/configure-payload-vps](../coolify/configure-payload-vps.md)

## Deploy / Redeploy Payload Application
- !ATTENTION! Deploy operation will DELETE all content. 
- TODO: Implement Backup/Restore operations before/after deployment.
- Coolify UI on VPS (https://coolify.< domain-name >)
  - Projects : < coolify-project-name > -> Applications : < coolify-application-name >
    - Stop service if it is already running. 
    - Reload Compose File
    - Save
    - Advanced -> Force deploy (without cache)
  - Wait until the message "Container payload-... Started"
    - Click debug icon for more build information
  - Check the green label above stays green "Running(healthy) for a couple of ten seconds...
  - After deployment node will build Payload CMS. It takes time (3 - 4 minutes).

## Set Traefik for the new application (VPS) [coolify/set-traefik-for-new-app-vps](../coolify/set-traefik-for-new-app-vps.md)

## Check the Application
- Check `https://www.< domain-name >`
  - Works, good `:)` .
  - Not working, bad `:(`, try [troubleshooting](#troubleshooting).

- Click `Visit the admin dashboard`
  - `https://www.< domain-name >/admin`
  - Create your first user, or login
  - Click "Seed your database", wait ...
    - If successful we get the `Database seeded! You can now visit your website` message.
      - Or just a "done" prompt right of "Seed your database" link.
    - If process fails check for file permissions and ownership in payload service container. For more information read troubleshooting below.

- Visit `https://www.< domain-name >`

- Page renders with images.

- Learn more ... [https://payloadcms.com/docs/getting-started/what-is-payload](https://payloadcms.com/docs/getting-started/what-is-payload)

---

### Production Environment: [production](../production.md#production-environment)

### Back to [README](../../README.md)

## References
- https://github.com/coollabsio/coolify-examples/tree/v4.x
- https://coolify.io/self-hosted/
- https://coolify.io/docs/knowledge-base/docker/compose#connect-to-predefined-networks
- https://coolify.io/docs/knowledge-base/proxy/traefik/redirects
- https://doc.traefik.io/traefik/reference/routing-configuration/http/middlewares/redirectregex/
- https://payloadcms.com/docs/production/deployment
- Payload Website template, https://github.com/payloadcms/payload/tree/main/templates/website .
- https://payloadcms.com/docs/getting-started/what-is-payload

---

## Troubleshooting

### Container restarts
  [Click](../troubleshoot/container-restarts.md) for details

### Build failed because of webpack errors
  Error: ENOSPC: no space left on device, write
  - Check for available free disk space on VM/VPS
  - Use Coolify UI, Stop Payload App, check clean, then deploy Payload App


### Error after checking "Make it publicly available"
Bind for 0.0.0.0:27017 failed: port is already allocated

- Coolify UI -> Projects -> payload (production)
  - Configuration -> Traefik (Coolify Proxy)
    - Remove 27017 port from ports
    - Save
  - Restart Proxy

### Error: #1 [internal] load local bake definitions

```bash
corepack enable pnpm && pnpm install --frozen-lockfile;
.....
Error type: App\Exceptions\DeploymentException
Error code: 0
Location: /var/www/html/app/Traits/ExecuteRemoteCommand.php:242
```

- [Apply troubleshoot/frozen-lockfile](../troubleshoot/frozen-lockfile.md) 
- [Apply troubleshoot/vps-disk-space](../troubleshoot/vps-disk-space.md) 

