During [Install npm packages & build the project](../payload/publish-payload-app.md#install-npm-packages--build-the-project) page renders then turns all white.

DevTools

Download the React DevTools for a better development experience: https://react.dev/link/react-devtools

Uncaught Error: Invalid src prop (http://localhost:3000/api/media/file/website-template-OG-2.webp?2026-01-30T23%3A02%3A01.620Z) on `next/image`, hostname "localhost" is not configured under images in your `next.config.js`

Edit .env file, change NEXT_PUBLIC_SERVER_URL=http://localhost:3000 

Save & exit

Stop node 

pnpm build

pnpm dev

