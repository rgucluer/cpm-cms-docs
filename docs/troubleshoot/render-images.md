# Payload does not render images

After initial seeding after deployment, Payload CMS does not render images in homepage.

Current (2026-04-28) images render on VM/VPS but not run on local development run (pnpm dev). Problem is solved after moving node build stage out of Dockerfile.


---

src/payload.config.ts
```javascript
.....
export default buildConfig({
  .....
  serverURL: process.env.NEXT_PUBLIC_SERVER_URL || 'http://localhost:3000',
  .....
})
```

---

- < payload-app-full-path >/next.config.ts

```javascript
import { withPayload } from '@payloadcms/next/withPayload'
import type { NextConfig } from 'next'
import path from 'path'
import { fileURLToPath } from 'url'

const __filename = fileURLToPath(import.meta.url)
const dirname = path.dirname(__filename)
import { redirects } from './redirects'

const NEXT_PUBLIC_SERVER_URL = process.env.NEXT_PUBLIC_SERVER_URL
  ? process.env.NEXT_PUBLIC_SERVER_URL
  : 'http://localhost:3000'

const nextConfig: NextConfig = {
  sassOptions: {
    loadPaths: ['./node_modules/@payloadcms/ui/dist/scss/'],
  },
  images: {
    localPatterns: [
      {
        pathname: '/api/media/file/**',
      },
    ],
    qualities: [100],
    remotePatterns: [
      ...[NEXT_PUBLIC_SERVER_URL].map((item) => {
        const url = new URL(item)

        return {
          hostname: url.hostname,
          protocol: url.protocol.replace(':', '') as 'http' | 'https',
        }
      }),
    ],
  },
  webpack: (webpackConfig) => {
    webpackConfig.resolve.extensionAlias = {
      '.cjs': ['.cts', '.cjs'],
      '.js': ['.ts', '.tsx', '.js', '.jsx'],
      '.mjs': ['.mts', '.mjs'],
    }

    return webpackConfig
  },
  experimental: {
    webpackMemoryOptimizations: true,
  },
  output: 'standalone',
  reactStrictMode: true,
  redirects,
  turbopack: {
    root: path.resolve(dirname),
  },
}

export default withPayload(nextConfig, { devBundleServerPackages: false })

```

---


## References:
- https://payloadcms.com/docs/configuration/overview
- https://nextjs.org/docs/app/api-reference/components/image#remotepatterns
- https://nextjs.org/docs/app/api-reference/config/next-config-js/output
- https://nextjs.org/docs/app/api-reference/config/next-config-js/allowedDevOrigins
- https://nextjs.org/docs/messages/next-image-unconfigured-host
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map
- https://dev.to/aaronksaunders/display-images-in-your-payload-cms-admin-collection-list-with-custom-cell-component-8f9

