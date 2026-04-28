# Publish a Payload CMS on a Virtual Private Server

We will use the Payload Website template, https://github.com/payloadcms/payload/tree/main/templates/website .

https://payloadcms.com/docs/getting-started/what-is-payload

https://payloadcms.com/docs/production/deployment

## On VPS

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

We created the files during development stage. Merge development to main branch.

On Developer PC
```bash
cd < workspace-full-path >/payload-app
```

https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging

```bash
git checkout main
```

```bash
git merge dev
```

```bash
git push origin main
```

## Coolify - Add a new Source ( Virtual Private Server )
[Click](../coolify/add-new-source-vps.md) for details

## Add Source to the Project (VPS)
[Click](../coolify/add-source-to-project-vps.md) for details

## Configure the Payload Project (VPS)
[Click](../coolify/configure-payload-vps.md) for details

## Deploy / Redeploy Payload Application
- Coolify UI on VPS (https://coolify.< domain-name >)
  - Projects -> < project-name > (production) -> payload 
    - Redeploy / Deploy
    - Or, Advanced -> Force deploy (without cache)
  - Wait until the message "Container payload-... Started"
    - Click debug icon for more build information
  - Check the green label above stays green "Running(healthy) for a couple of ten seconds...

## Set Traefik for the new application (VPS)
[Click](../coolify/set-traefik-for-new-app-vps.md) for details


## Check the Application
- Check `https://www.< domain-name >`
  - Works, good `:)` .
  - Not working, bad `:(`, try [troubleshooting](#troubleshooting).

- Click `Visit the admin dashboard`
  - Create your first user
    - Create or Login
  - Click "Seed your database", wait ...
    - If successful we get the `Database seeded! You can now visit your website` message.
    - If process fails check for file permissions and ownership in payload service container. For more information read troubleshooting below.

- Visit `https://www.< domain-name >`

- Page renders with images.

- Learn more ... [https://payloadcms.com/docs/getting-started/what-is-payload](https://payloadcms.com/docs/getting-started/what-is-payload)

## References
- https://github.com/coollabsio/coolify-examples/tree/v4.x
- https://coolify.io/self-hosted/
- https://coolify.io/docs/knowledge-base/docker/compose#connect-to-predefined-networks
- https://coolify.io/docs/knowledge-base/proxy/traefik/redirects
- https://doc.traefik.io/traefik/reference/routing-configuration/http/middlewares/redirectregex/
- https://payloadcms.com/docs/production/deployment

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


