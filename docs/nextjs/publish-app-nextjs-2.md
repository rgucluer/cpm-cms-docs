## [Create a new private respository on GitHub](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)

GitHub Repository name: next-ssr

```bash
cd <workspace-full-path>
```

```bash
git clone https://github.com/<github-username>/next-ssr
```

If repository is private, use ssh link.

### Copy content of example repository to local directory

```bash
cd <workspace-full-path>
```

```bash
git clone https://github.com/coollabsio/coolify-examples.git 
```

Copy content of <workspace-full-path>/coolify-examples/nextjs/ssr/ to <workspace-full-path>/next-ssr

```bash
cd <workspace-full-path>/next-ssr
```

Install using your package manager. (npm, yarn, pnpm, bun ). Use only one package manager. Before switching to another package manager, delete existing lock files. Modify code as you wish.

```bash
corepack use pnpm@latest-10
```

```bash
pnpm approve-builds
```

```bash
pnpm install
```

```bash
pnpm build
```

If it works, commit the code to the repository.

```bash
git add .
```

```bash
git commit
```

Enter "Initial commit. Cloned from Coolify Examples Next.js ssr example."
Save & Exit

```bash
git push origin main
```

Check status of repository on Github .

Start VM.

```bash
multipass start coolvm
```

Open Coolify https://coolify.<dev-domain-name>

## Coolify - Remove Resource

- Coolify UI -> Projects -> cpm-cms (production) -> payload
  - Danger Zone
    - Delete
      - Confirm Resource Deletion ?
        - Continue
          - Enter Resource Name: payload
            - Continue
              - Confirm Resource Deletion ?
                - Enter your password, and confirm.

- Remove cpm-cms project
- Add new project: next-ssr

## Coolify - Add Source
- Coolify UI -> Sources
  - Add
    - Name : next-ssr
    - Continue
    - Automated Installation
      - Webhook Endpoint
        - Select from select box: Use https://coolify.devserver1.my-domain.com
        - Register Now
        - Confirm Access ...
        - GitHub App name: next-ssr
          - Create GitHub App for <github-username>
            - You must complete this step before you can use this source!
              - Click Install Repositories on GitHub
                - Install next-ssr
                  - Only select repositories
                    - Select <github-username>/next-ssr
                    - Install
        - GitHub App
          - Save

## Coolify - Add Resource

- Coolify UI -> Projects -> next-ssr (production) -> Add Resource
  - Private Repository (with GitHub App)
    - Select a GitHub App
      - next-ssr
        - Load Repository
          - Branch: main
          - Build Pack: Docker Compose
          - Base Directory : /
          - Docker Compose Location: /prod-docker-compose.yaml
          - Continue
            - Opens Configuration

- Configuration: Coolify UI -> Projects -> next-ssr (production) -> nextjs
  - General
    - General
      - Name: nextjs
      - Build Pack: Docker Compose
      - Domains: https://www.devserver1.my-domain.com
    - Build
      - Base Directory: /
      - Docker Compose Location: /prod-docker-compose.yaml
      - Check : Preserve Repository During Deployment
  - Save

- Environment Variables
  - Add
    - Name: NEXT_PUBLIC_BASE_URL
      - Value: https://www.devserver1.my-domain.com
      - Save
    - Close the form

- Resource Limits
  - Limit CPUs ( https://docs.docker.com/engine/containers/run/#cpu-share-constraint )
    - Number of CPUs: 0.5
      - Number is a fractional number. 0.000 means no limit.
    - CPU sets to use: Empty
      - CPUs in which to allow execution (0-3, 0,1)
    - CPU Weight: 512
    - Soft Memory Limit: 3g
    - Maximum Memory Limit: 3g
    - Swappiness: 1
    - Maximum Swap Limit: 3g
    - Save
    - ( Set Memory values to according to your setup. )

- Coolify UI -> Projects -> next-ssr (production) -> nextjs
  - Deploy/Redeploy
  - Deploy takes time.
    - Click Show Debug Logs
    - Watch for "New container started." message.
    - Check https://www.devserver1.my-domain.com
    - Works, good `:)` .



## References
- https://github.com/coollabsio/coolify-examples/tree/v4.x
- https://github.com/coollabsio/coolify-examples/tree/v4.x/nextjs/ssr 
- https://github.com/coollabsio/coolify-examples/tree/v4.x/nextjs/spa
- https://coolify.io/self-hosted/
