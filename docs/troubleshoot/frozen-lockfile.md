On Dev PC

Run Virtual Machine, database service must be runnning

```bash
$ cd < payload-app-full-path >
```

```bash
$ rm -rf ./node_modules/ ./.next pnpm-lock.yaml
```

```bash
$ pnpm store prune
```

```bash
$ corepack use pnpm@latest-11
```

```bash
$ pnpm install
```

```bash
$ pnpm build
```

```bash
$ git add .
```

```bash
$ git commit
```

Depending on your current branch
```bash
$ git push origin dev
```
or
```bash
$ git push origin main
```

- Coolify UI (On VM) -> Projects -> cpm-cms (production) -> payload(devserver1) -> Redeploy
or
- Coolify UI (On VM) -> Projects -> cpm-cms (production) -> payload(devserver1) -> Advanced Forced Deploy

- Wait 3-4 minutes after deployment finishes.

- Open https://www.< dev-domain-name >/admin
  - Login
  - Click Seed your database
    - We do this after a deployment
    - TODO: Implement a backup/restore process to persist existing content between deployments.
    - Open https://www.< dev-domain-name >
    - Home page renders with images and default theme

