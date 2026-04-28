## Make some changes in source code

Edit < workspace-full-path >/.gitignore file . Add "*.db" at the end of file . Save & exit .
```text
.....
/playwright/.cache/

*.db
*.old
```

---

< workspace-full-path >/Dockerfile

Original:
https://github.com/payloadcms/payload/blob/main/templates/website/Dockerfile

Changed:
https://github.com/rgucluer/cpm-cms/blob/main/Dockerfile

---

< workspace-full-path >/docker-compose.yml

Original:
https://github.com/payloadcms/payload/blob/main/templates/website/docker-compose.yml

Changed:
https://github.com/rgucluer/cpm-cms/blob/main/docker-compose.yml

---

< workspace-full-path >/next.config.js

Original:
https://github.com/payloadcms/payload/blob/main/templates/website/next.config.ts

Changed:
https://github.com/rgucluer/cpm-cms/blob/main/next.config.ts

---

If needed update pnpm. 

```bash
corepack use pnpm@latest-10
```

< workspace-full-path >/package.json

Our previous actions make the following changes in package.json

< workspace-full-path >/next.config.js

Original:
https://github.com/payloadcms/payload/blob/main/templates/website/package.json

Changed:
https://github.com/rgucluer/cpm-cms/blob/main/package.json

---

pnpm-lock.yaml is generated with `pnpm install` command referencing package.json content. If error occurs after installing and removing npm packages apply steps in [frozen-lockfile](../troubleshoot/frozen-lockfile.md)

---

< workspace-full-path >/postcss.config.js
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

< workspace-full-path >/payload-app/src/payload.config.ts
```javascript
  .....
  serverURL: process.env.NEXT_PUBLIC_SERVER_URL || 'http://localhost:3000',
  collections: [Pages, Posts, Media, Categories, Users],
  .....
```

< workspace-full-path >/payload-app/.env
```ini
NODE_OPTIONS="--no-deprecation --max-old-space-size=3072"

HOSTNAME='0.0.0.0'
.....
```
Enter a value for CRON_SECRET, and PREVIEW_SECRET


Continue from [Install npm packages & build the project](publish-payload-app.md#install-npm-packages--build-the-project)