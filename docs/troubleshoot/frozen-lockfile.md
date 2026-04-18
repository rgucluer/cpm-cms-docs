On Dev PC
```bash
cd <payload-app-full-path>
```

```bash
rm -rf ./node_modules/ ./.next pnpm-lock.yaml
```

```bash
pnpm store prune
```

```bash
pnpm install
```

```bash
pnpm build
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
or
```bash
git push origin main
```

Depending on your current branch


- Coolify UI (On VM) -> Projects -> cpm-cms (production) -> payload(devserver1) -> Redeploy
or
- Coolify UI (On VM) -> Projects -> cpm-cms (production) -> payload(devserver1) -> Advanced Forced Deploy

