# Publish Payload CMS on a Virtual Private Server

We will use the Payload Website template, https://github.com/payloadcms/payload/tree/main/templates/website .

https://payloadcms.com/docs/getting-started/what-is-payload

https://payloadcms.com/docs/production/deployment

## Open Coolify Web UI of Virtual Private Server

- Visit `https://coolify.< domain-name >`

## Coolify - Add a new Project

- Coolify UI -> Projects -> Add
  - New Project
    - Name: payload-project
    - Description: Payload template (website) for Coolify
    - Continue

## Create a MongoDB using Coolify

- Coolify UI -> Projects -> payload-project (production) 
  - Resources
    - Add Resource (or +New )
    - Databases
      - Mongo DB 
        - Configuration:
          - Name: mongodb-payload-vps
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
        - Start/Restart
          - Close "Database Startup" form after "Database started." message
          - Check for green Running (healthy) label

## Create a local copy of Payload CMS Website template

We created the files during development stage. Merge development to main branch. Combine with squash merge.

https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging

https://git-scm.com/cheat-sheet

On Developer PC

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

## Coolify - Add a new Source ( Virtual Private Server )
Apply [coolify/add-new-source-vps](../coolify/add-new-source-vps.md)

## Add Source to the Project (VPS)
Apply [coolify/add-source-to-project-vps](../coolify/add-source-to-project-vps.md)

## Configure the Payload Project (VPS)
Apply [coolify/configure-payload-vps](../coolify/configure-payload-vps.md)

## Deploy / Redeploy Payload Application
- !ATTENTION! Deploy operation will delete all content. 
- TODO: Implement Backup/Restore operations before/after deployment.
- Coolify UI on VPS (https://coolify.< domain-name >)
  - Projects -> < project-name > (production) -> payload 
    - Stop service if it is already running. 
    - Deploy / Redeploy
    - Or, Advanced -> Force deploy (without cache)
  - Wait until the message "Container payload-... Started"
    - Click debug icon for more build information
  - Check the green label above stays green "Running(healthy) for a couple of ten seconds...
  - After deployment node will build Payload CMS. It takes time (3 - 4 minutes).

## Set Traefik for the new application (VPS)
Apply [coolify/set-traefik-for-new-app-vps](../coolify/set-traefik-for-new-app-vps.md)

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

- Back to [README](../../README.md)

- [Daily Production Operations](../daily-prod.md)

## References
- https://github.com/coollabsio/coolify-examples/tree/v4.x
- https://coolify.io/self-hosted/
- https://coolify.io/docs/knowledge-base/docker/compose#connect-to-predefined-networks
- https://coolify.io/docs/knowledge-base/proxy/traefik/redirects
- https://doc.traefik.io/traefik/reference/routing-configuration/http/middlewares/redirectregex/
- https://payloadcms.com/docs/production/deployment

---

## TODO: Work on better content persistence

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

- [Apply troubleshoot/vps-disk-space](../troubleshoot/vps-disk-space.md) 
- [Apply troubleshoot/frozen-lockfile](../troubleshoot/frozen-lockfile.md) 


