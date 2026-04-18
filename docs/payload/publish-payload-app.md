# Publish a Payload CMS on a Virtual Machine

We will use the Payload Website template, https://github.com/payloadcms/payload/tree/main/templates/website .

https://payloadcms.com/docs/getting-started/what-is-payload

https://payloadcms.com/docs/production/deployment

## Open Coolify Web UI of Virtual Machine

- Start Virtual Machine
- Visit `https://coolify.devserver1.<domain-name>`

## Coolify - Add a new Project

- Coolify Web UI -> Projects -> Add
  - New Project
    - Name: cpm-cms
    - Description: Payload website template for Coolify
    - Continue

## Create a MongoDB using Coolify
[Click](create-mongodb.md) for details

## Create a Payload CMS Application
[Click](create-payload-cms.md) for details

## Create a new private respository on GitHub
[Click](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository) for details

Private repository name: cpm-cms

## Make some changes in source code
[Click](change-source-code.md) for details

## Install npm packages & build the project

```bash
cd <payload-app-full-path>
```

Check values in .env file, if it fits your current setup.

```bash
pnpm install
```

```bash
pnpm build
```

### Run application in development mode

Edit next.config.ts, if exists comment out ```output``` save & exit.

```bash
pnpm dev
```

This command uses the .env file for environment variables. When we use Coolify inside a VM or VPS we define each variable in the Coolify UI Environment Variables page.

```bash
> payload-app@1.0.0 dev <payload-app-full-path>
> cross-env NODE_OPTIONS=--no-deprecation next dev
or
> next dev

   ▲ Next.js 15.4.11
   - Local:        http://localhost:3000
   - Network:      http://<dev-pc-local-ip>:3000
   - Environments: .env
   - Experiments (use with caution):
     ✓ webpackMemoryOptimizations

 ✓ Starting...
 ✓ Ready in 3.6s
```

Check, and fix any errors.

Open http://localhost:3000 in a browser

<kbd>CTRL</kbd> + <kbd>C</kbd> in terminal to stop the server

---

### Run application in production mode on local machine
cd <payload-app-full-path>

Edit next.config.ts, add ```output``` as following, save & exit.

```typescript
.....
const nextConfig: NextConfig = {
  .....
  output: 'standalone',
  reactStrictMode: true,
  redirects,
  .....
}

export default withPayload(....
```

```bash
pnpm install
```

```bash
pnpm build
```

Copy <payload-app-full-path>/public into <payload-app-full-path>/.next/standalone

Copy <payload-app-full-path>/.next/static into <payload-app-full-path>/.next/standalone/.next

```bash
node .next/standalone/server.js
```

Open a web browser, navigate to http://localhost:3000

This renders the homepage of Payload.

```CTRL``` + ```C``` on terminal to stop the service

---

## Add a new remote for cpm-cms

Set a GitHub repository as the remote for cpm-cms

https://docs.github.com/en/get-started/git-basics/managing-remote-repositories

```bash
cd <workspace-full-path>/payload-app
```

```bash
git remote add origin git@github.com:<github-username>/cpm-cms.git
```

## git push code to GitHub repository
[Click](../git/git-push-dev.md) for details


## Create a new branch "dev" on Github

https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository


### Git push to dev branch

After we make changes to source code, and commit the changes we can push the code to the original repo on GitHub.

https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-branches

```bash
git checkout -b dev
```

```bash
git push origin dev
```

## Coolify - Add a new Source
[Click](../coolify/add-new-source.md) for details


## Add Source to the Project
[Click](../coolify/add-source-to-project.md) for details


## Configure the Payload Project
[Click](../coolify/configure-payload-dev.md) for details


## Deploy / Redeploy Payload Application
- Coolify UI on VM (https://coolify.devserver1.my-domain.com)
  - Projects -> <project-name> (production) -> payload -> 
    - Redeploy / Deploy
    - Or, Advanced -> Force deploy (without cache)
  - Wait until the message "Container payload-... Started"
    - Click debug icon for more build information
  - Check the green label above stays green "Running(healthy) for a couple of seconds...

## Set Traefik for the new application
[Click](../coolify/set-traefik-for-new-app.md) for details


## Check the Application
- Check `https://www.devserver1.<domain-name>`
  - Works, good `:)` .
  - Not working, bad `:(`, try [troubleshooting](#troubleshooting).

- Click `Visit the admin dashboard`
  - Create your first user
    - Create
  - or Login

- Seed your database
  - Click `Seed your database` link
    - If successful we get the `Database seeded! You can now visit your website` message.
    - If process fails check for file permissions and ownership in payload service container. For more information read troubleshooting below.

- Visit `https://www.devserver1.<domain-name>`

- Page renders with images.

- Learn more ... [https://payloadcms.com/docs/getting-started/what-is-payload](https://payloadcms.com/docs/getting-started/what-is-payload)


## Continue with
- [Production Environment](docs/production.md)


## References
- https://payloadcms.com/docs/production/deployment
- https://github.com/coollabsio/coolify-examples/tree/v4.x/nextjs/ssr 
- https://github.com/coollabsio/coolify-examples/tree/v4.x/nextjs/spa
- https://coolify.io/self-hosted/
- https://coolify.io/docs/knowledge-base/proxy/traefik/redirects
- https://coolify.io/docs/knowledge-base/docker/compose


## Troubleshooting

Currently working on:
### Payload does not render images
- [Click](../troubleshoot/render-images.md) to see the solution.

### Payload build error
- ERR_PNPM_OUTDATED_LOCKFILE  Cannot install with "frozen-lockfile" because pnpm-lock.yaml is not up to date with <ROOT>/package.json
- [Click](../troubleshoot/frozen-lockfile.md) to see the solution.

### Error: It looks like you're trying to use `tailwindcss` directly as a PostCSS plugin
- [Click](../troubleshoot/tailwindcss.md) to see the solution.

### Page turns white
- [Click](../troubleshoot/page-turns-white.md) to see the solution.

### SSL Certificate Error net::ERR_CERT_AUTHORITY_INVALID

- Check Server Traefik configuration
  - <payload-app-full-path>/docs/coolify/configure-coolify-traefik.md
- Check Application Traefik configuration
- Make necessary changes, restart Proxy, Redeploy application

### Seeding error

- Coolify UI -> Projects -> cpm-cms -> Applications: payload -> Terminal
  - ```bash
    cd ~
    ```
  
  - ```bash
    ls -la
    ```
  
    ```bash
    total 24
    drwxr-sr-x    1 node     node          4096 Apr  3 12:04 .
    drwxr-xr-x    1 root     root          4096 Mar 26 05:36 ..
    -rw-------    1 node     node            62 Apr  3 12:05 .ash_history
    drwxr-sr-x    3 node     node          4096 Apr  2 18:56 .config
    drwxr-sr-x    9 node     node          4096 Apr  2 09:10 app
    ```

    If app directory is owned by node user, Seeding would work properly. If it is owned by root user than errors can occur. In that case check Dockerfile for file/directory copy, and ownership operations .

### pnpm dev ends in infinite loop, FATAL: An unexpected Turbopack error occurred.

Log contains "Next.js package not found"

- [Click](../troubleshoot/frozen-lockfile.md) to see the solution.

