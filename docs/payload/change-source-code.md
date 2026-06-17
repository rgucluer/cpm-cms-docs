## Make some changes in source code


< payload-app-full-path >/src/components/Media/ImageMedia/index.tsx
```typescript
.....
  return (
    <picture className={cn(pictureClassName)}>
      <NextImage
        unoptimized
        alt={alt || ''}
        .....
      />
.....
```

---

< payload-app-full-path >/src/payload.config.ts
```javascript
  .....
  serverURL: process.env.NEXT_PUBLIC_SERVER_URL || 'http://localhost:3000',
  collections: [
  .....
  ],
  .....
```

---

< payload-app-full-path >/next.config.ts
```typescript

const NEXT_PUBLIC_SERVER_URL = process.env.NEXT_PUBLIC_SERVER_URL
  ? process.env.NEXT_PUBLIC_SERVER_URL
  : 'http://localhost:3000'

const nextConfig: NextConfig = {
  allowedDevOrigins: ['127.0.0.1', '::1', 'localhost'],
  .....
  images: {.....},
  webpack: {.....},
  experimental: {
    webpackMemoryOptimizations: true,
  },
  output: 'standalone',
  .....
}

export default .....
```

---

< payload-app-full-path >/.env

< payload-app-full-path >/.env.example

```ini
NODE_OPTIONS="--no-deprecation --max-old-space-size=3072"

HOSTNAME='0.0.0.0'

.....
```

< payload-app-full-path >/.env

Enter a value for CRON_SECRET, and PREVIEW_SECRET

---

Edit < payload-app-full-path >/.gitignore file . Add "*.db" at the end of file . Save & exit .
```text
.....
/playwright/.cache/

*.db
*.old
*.ignore
```

---

< payload-app-full-path >/Dockerfile

Original:
https://github.com/payloadcms/payload/blob/main/templates/website/Dockerfile

Changed:
https://github.com/rgucluer/cpm-cms/blob/dev/Dockerfile

---

< payload-app-full-path >/docker-compose.yml

Original:
https://github.com/payloadcms/payload/blob/main/templates/website/docker-compose.yml

Changed:
https://github.com/rgucluer/cpm-cms/blob/dev/docker-compose.yml

---

pnpm-lock.yaml is generated with `pnpm install` command or `corepack use pnpm@latest-10` command referencing package.json content. We will use this command in the next section. If error occurs after installing and removing npm packages apply steps in [frozen-lockfile](../troubleshoot/frozen-lockfile.md).

---

Install/update pnpm. 

```bash
cd < payload-app-full-path >
```
```bash
corepack use pnpm@latest-10
```

---

Our previous actions make the following changes in package.json

Original:
https://github.com/payloadcms/payload/blob/main/templates/website/package.json

Changed:
https://github.com/rgucluer/cpm-cms/blob/dev/package.json

---

< payload-app-full-path >/postcss.config.js

```javascript
import autoprefixer from "autoprefixer"

const config = {
  plugins: {
    '@tailwindcss/postcss': {},
    autoprefixer: {},
  },
}

export default config

```

---

< payload-app-full-path >/src/payload-css.d.ts
```typescript
declare module '@payloadcms/next/css'
```

---

after-deploy.sh
```bash
export NODE_ENV=production
# export NEXT_TELEMETRY_DISABLED=1

cd /app

if [ -f pnpm-lock.yaml ]; then corepack enable pnpm && pnpm run build; \
else echo "Lockfile not found." && exit 1; \
fi

cp -r /app/public /home/node/app/
cp -r /app/.next/standalone/. /home/node/app/

mkdir /home/node/app/.next
chown node:node /home/node/app/.next
cp -r /app/.next/static /home/node/app/.next/static

chown -R node:node /home/node/app

su - node

cd /home/node/app

# server.js is created by next build from the standalone output
# https://nextjs.org/docs/pages/api-reference/next-config-js/output

node server.js
```

---

Continue from [Install npm packages & build the project](publish-payload-app.md#install-npm-packages--build-the-project)
