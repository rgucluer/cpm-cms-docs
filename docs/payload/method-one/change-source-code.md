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
.....
import { redirects } from './redirects'

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
  // output: 'standalone',
  .....
}

export default .....
```

---

< payload-app-full-path >/.env

< payload-app-full-path >/.env.example

```ini
NODE_OPTIONS="--no-deprecation --max-old-space-size=2048"

HOSTNAME='0.0.0.0'

.....
```

---

< payload-app-full-path >/.env

Enter a value for CRON_SECRET, PREVIEW_SECRET

Check DATABASE_URL , if this is not set, set it to Database public URL, Change IP to VM IP

---

Edit < payload-app-full-path >/.gitignore file . Add "*.db" at the end of file . Save & exit .
```text
.....
/playwright/.cache/

*.db
```

---

< payload-app-full-path >/Dockerfile

Original:
https://github.com/payloadcms/payload/blob/v3.86.0/templates/website/Dockerfile

Changed:
https://github.com/rgucluer/cpm-cms/blob/main/Dockerfile

---

< payload-app-full-path >/docker-compose.yml

Original:
https://github.com/payloadcms/payload/blob/v3.86.0/templates/website/docker-compose.yml

Changed:
https://github.com/rgucluer/cpm-cms/blob/main/docker-compose.yml

---

pnpm-lock.yaml is generated with `pnpm install` command or `corepack use pnpm@latest-11` command referencing package.json content. We will use this command in the next section. If error occurs after installing and removing npm packages apply steps in [frozen-lockfile](../troubleshoot/frozen-lockfile.md).

---

package.json

- Remove the last pnpm section.


Our previous actions make the following changes in package.json

Original:
https://github.com/payloadcms/payload/blob/v3.86.0/templates/website/package.json

Changed:
https://github.com/rgucluer/cpm-cms/blob/main/package.json

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

Add LICENSE.md

---

Rename README.md as README-payload.md

---

Add README.md

---

< payload-app-full-path >/after-deploy.sh

```bash
export NODE_ENV=production
# export NEXT_TELEMETRY_DISABLED=1

cd /app

corepack enable

corepack enable pnpm && corepack prepare pnpm@latest-11 --activate

if [ -f pnpm-lock.yaml ]; then pnpm run build; \
else echo "Lockfile not found." && exit 1; \
fi

cp -r /app/public /home/node/app/
cp -r /app/.next/standalone/. /home/node/app/

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

### Continue with: Install npm packages & build the project [payload/method-one/install-npm-packs-build](create-payload-cms-m1#install-npm-packages--build-the-project-payloadmethod-oneinstall-npm-packs-build)
