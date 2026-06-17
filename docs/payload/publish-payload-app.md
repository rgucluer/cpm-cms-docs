# Publish a Payload CMS on a Virtual Machine

We will use the Payload Website template, https://github.com/payloadcms/payload/tree/main/templates/website .

https://payloadcms.com/docs/getting-started/what-is-payload

https://payloadcms.com/docs/production/deployment

## Open Coolify Web UI of Virtual Machine

- Start Virtual Machine
- Visit `https://coolify.devserver1.< domain-name >`

## Coolify - Add a new Project

- Coolify Web UI -> Projects -> Add
  - New Project
    - Name: cpm-cms
    - Description: Payload website template for Coolify
    - Continue

## Create a MongoDB using Coolify
Apply [create-mongodb](create-mongodb.md)

## Install nvm, and Node
Apply [nextjs/install-nvm-node](nextjs/install-nvm-node.md)

## Create a Payload CMS Application
- Method One:
  - If you are creating a new Payload CMS application, and do not committed to a git repository yet, use this method.
  - [Create app with pnpx](create-payload-cms.md)
- Method Two: If you already have a git repository you created before, use this method.
  - [Clone an existing git repository](create-payload-cms-with-git-clone.md) .

## Make some changes in source code
Apply [change-source-code](change-source-code.md)

## Install npm packages & build the project

On Developer PC
```bash
cd < payload-app-full-path >
```

Check values in .env file, if it fits your current setup.

```bash
pnpm install
```

### Run application in development mode

```bash
pnpm dev
```

This command uses the .env file for environment variables. When we use Coolify inside a VM or VPS we define each variable in the Coolify UI Environment Variables page.

Wait for results ...

```bash
> payload-app@0.1.0 dev < workspace-full-path >/payload-app
> cross-env NODE_OPTIONS=--no-deprecation next dev

▲ Next.js 16.2.3 (Turbopack)
- Local:        http://localhost:3000
- Network:      < dev-pc-local-ip >:3000
- Environments: .env
✓ Ready in 374ms
- Experiments (use with caution):
  ⨯ turbopackServerFastRefresh
  ✓ webpackMemoryOptimizations

```
- Check, and fix any errors.
- Open http://localhost:3000 in a browser
  - This renders Payload CMS 

<kbd>CTRL</kbd> + <kbd>C</kbd> in terminal to stop the server

---

```bash
pnpm build
```
---

## Create a new private respository on GitHub
Read [Github creating-a-new-repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository) for details

## Add a new remote for payload-app

```bash
cd < workspace-full-path >/payload-app
```

Check if a remote is defined:
```bash
git config list
```

Look for `remote.origin.url`

If a remote does not exist, set a GitHub repository as the remote for cpm-cms

https://docs.github.com/en/get-started/git-basics/managing-remote-repositories


```bash
git remote add origin git@github.com:< github-username >/cpm-cms.git
```

## git push code to GitHub repository
[Click](../git/git-push.md) for details

## Create a new branch "dev" on Github

https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository


### Git push to dev branch

```bash
git checkout -b dev
```

After we make changes to source code, and commit the changes we can push the code to the original repo on GitHub.

https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-branches


```bash
git push origin dev
```

## Coolify - Add a new Source ( Virtual Machine )
Apply [coolify/add-new-source](../coolify/add-new-source.md)

## Add Source to the Project
Apply [coolify/add-source-to-project](../coolify/add-source-to-project.md) for details

## Configure the Payload Project
Apply [coolify/configure-payload-dev](../coolify/configure-payload-dev.md) for details

## Deploy / Redeploy Payload Application
- !ATTENTION! Deploy operation will delete all content. 
  - TODO: Implement Backup/Restore operations before/after deployment.
- Coolify UI on VM (https://coolify.devserver1.my-domain.com)
  - Projects -> < project-name (production) -> payload
    - Stop service if it is already running. 
    - Deploy / Redeploy
    - Or, Advanced -> Force deploy (without cache)
  - Wait until the message "Container payload-... Started"
    - Click debug icon for more build information
  - Check the green label above stays green "Running(healthy) for a couple of ten seconds...
  - After deployment node will build Payload CMS. It takes time (3 - 4 minutes).
  

## Set Traefik for the new application
Apply [coolify/set-traefik-for-new-app](../coolify/set-traefik-for-new-app.md)

## Check the Application
- Check `https://www.devserver1.< domain-name >`
  - Works, good `:)` .
  - Not working, bad `:(`, try [troubleshooting](#troubleshooting).

- Click `Visit the admin dashboard`
  - `https://www.< domain-name >/admin`
  - Create your first user, or login
  - Click "Seed your database", wait ...
    - If successful we get the `Database seeded! You can now visit your website` message.
      - Or just a "done" prompt right of "Seed your database" link.
    - If process fails check for file permissions and ownership in payload service container. For more information read troubleshooting below.

- Visit `https://www.devserver1.< domain-name >`

- Page renders with images.

- Learn more ... [https://payloadcms.com/docs/getting-started/what-is-payload](https://payloadcms.com/docs/getting-started/what-is-payload)


## Continue with
- [Production Environment](docs/production.md)

- Back to [README](../../README.md)

- [Daily Development Operations](../daily-dev.md)


## References
- https://payloadcms.com/docs/production/deployment
- https://github.com/coollabsio/coolify-examples/tree/v4.x/nextjs/ssr 
- https://github.com/coollabsio/coolify-examples/tree/v4.x/nextjs/spa
- https://coolify.io/self-hosted/
- https://coolify.io/docs/knowledge-base/proxy/traefik/redirects
- https://coolify.io/docs/knowledge-base/docker/compose
- https://nextjs.org/docs/messages/next-image-unconfigured-localpatterns


## TODO: Work on better content persistence

---

## Troubleshooting

### Error: #1 [internal] load local bake definitions

```bash
corepack enable pnpm && pnpm install --frozen-lockfile;
.....
Error type: App\Exceptions\DeploymentException
Error code: 0
Location: /var/www/html/app/Traits/ExecuteRemoteCommand.php:242

```
- [Apply troubleshoot/frozen-lockfile](../troubleshoot/frozen-lockfile.md) 

### Payload does not render images
- In progress
- Apply [troubleshoot/render-images](../troubleshoot/render-images.md) 

### Error during seeding
- An error occured while seeding
  - Check Dockerfile for proper file/directory ownership
    - Example
      ```Dockerfile
      COPY --from=builder --chown=nextjs:nodejs /app/public ./public
      ```
      fixed my current error

### An error occured while seeding
- Apply [troubleshoot/seeding](../troubleshoot/seeding.md) for details

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
  - Apply [git/git-pull-1](../git/git-pull-1.md) for details 
