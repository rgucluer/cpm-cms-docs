### Error: It looks like you're trying to use `tailwindcss` directly as a PostCSS plugin

- <payload-app-full-path>/postcss.config.js
```js
const config = {
  plugins: {
    '@tailwindcss/postcss': {},
    autoprefixer: {},
  },
}

export default config

```

- Install PostCSS Plugin Package
```bash
cd <payload-app-full-path>
```
```bash
pnpm install -D @tailwindcss/postcss
```
- <payload-app-full-path>/src/app/(frontend)/globals.css

Change following

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
to
```css
@import "tailwindcss";
@config "../../../tailwind.config.mjs";
```
