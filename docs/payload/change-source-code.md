## Make some changes in source code

Edit <workspace-full-path>/.gitignore file . Add "*.db" at the end of file . Save & exit .
```text
.....
/playwright/.cache/

*.db
*.old
```

---

<workspace-full-path>/next.config.js
Original:
[https://github.com/payloadcms/payload/tree/main/templates/website](https://github.com/payloadcms/payload/tree/main/templates/website/next.config.ts)

Changed:
[https://github.com/rgucluer/cpm-cms/blob/main/next.config.js](https://github.com/rgucluer/cpm-cms/blob/main/next.config.js)

---

Add a npm package

```bash
cd <payload-app-full-path>
```

```bash
pnpm add tailwindcss-animate@1.0.7
```

If needed update pnpm. 

```bash
corepack use pnpm@latest-10
```

<workspace-full-path>/package.json

Our previous actions make the following changes in package.json

```javascript
{
  /* other code */

  "dependencies": {
  /* other code */
    "@payloadcms/db-mongodb": "3.81.0",
  /* other code */
    "tailwindcss-animate": "^1.0.7"
  },
  /* other code */
  "packageManager": "pnpm@....."
}
```


---

pnpm-lock.yaml is generated with `pnpm install` command referencing package.json content. If error occurs after installing and removing npm packages apply steps in [frozen-lockfile](../troubleshoot/frozen-lockfile.md)

---

<workspace-full-path>/postcss.config.js
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

<workspace-full-path>/payload-app/src/components/Media/ImageMedia/index.tsx

Set Image unoptimized

```javascript
.....
  if (!src && resource && typeof resource === 'object') {
    .....
    // src = getMediaUrl(url, cacheTag)
    src = `${url}`
  }
.....
  return (
    <picture className={cn(pictureClassName)}>
      <NextImage
        unoptimized
        .....
      />
    </picture>
  )
```

---

<workspace-full-path>/payload-app/src/payload.config.ts
```javascript
  .....
  serverURL: process.env.NEXT_PUBLIC_SERVER_URL || 'http://localhost:3000',
  collections: [Pages, Posts, Media, Categories, Users],
  .....
```

<workspace-full-path>/payload-app/.env
```ini
NODE_OPTIONS="--no-deprecation --max-old-space-size=3072"

HOSTNAME='0.0.0.0'
.....
```


Continue from [Install npm packages & build the project](publish-payload-app.md#install-npm-packages--build-the-project)