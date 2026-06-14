On Dev PC
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
$ corepack use pnpm@latest-10
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

