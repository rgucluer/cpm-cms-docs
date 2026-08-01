## Install npm packages & build the project

On Developer PC
```bash
$ cd < payload-app-full-path >
```

Check values in .env file, if it fits your current setup.

```bash
$ corepack enable
```

```bash
$ corepack enable pnpm && corepack use pnpm@latest-11
```

```bash
$ pnpm install
```

### Run application in development mode

```bash
$ pnpm dev
```

This command uses the .env file for environment variables. When we use Coolify inside a VM or VPS we define each variable in the `Coolify UI: Environment Variables` page.

Wait for results ...

```bash
▲ Next.js 16.2.6 (Turbopack)
- Local:        http://localhost:3000
- Network:      http://< dev-pc-local-ip >:3000
- Environments: .env
✓ Ready in 322ms
- Experiments (use with caution):
  ⨯ turbopackServerFastRefresh
  ✓ webpackMemoryOptimizations

```
- Check, and fix any errors.
- Open http://localhost:3000 in a browser
  - This renders Payload CMS without images

<kbd>CTRL</kbd> + <kbd>C</kbd> in terminal to stop the server

---

### Build application

< payload-app-full-path >/next.config.ts
```typescript
.....
const nextConfig: NextConfig = {

  experimental: {
    .....
  },
  output: 'standalone',
  .....
}
```

```bash
$ pnpm build
```

---

## For Method One (pnpx) Continue with: 
Create a new private respository on GitHub [payload/method-one/create-payload-cms-m1](../payload/method-one/create-payload-cms-m1.md#create-a-new-private-respository-on-github-external-github-creating-a-new-repository)

---

## For Method Two (git clone) continue with : 
Coolify - Add a new Source ( GitHub App ) too Coolify [coolify/add-new-source](../payload/publish-payload-app.md#add-a-new-source--github-app--to-coolify-coolifyadd-new-source)


---

### References:
- https://pnpm.io/settings
- https://nextjs.org/docs/app/guides/local-development
