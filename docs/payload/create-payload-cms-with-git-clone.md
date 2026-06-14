## Clone a Payload CMS Application

On Developer PC

```bash
$ cd < workspace-full-path >
```

Check a directory does not exist with the same name as the repository name.

```bash
$ git clone git@github.com:< github-username >/< git-repo-name >.git
```

```bash
$ mv cpm-cms payload-app
```

```bash
$ cd payload-app
```

```bash
$ corepack use pnpm@latest-10
```

```bash
$ pnpm approve-builds
```

```bash
$ pnpm install
```

```bash
$ cp .env.example .env
```

- Edit .env file
  - NODE_OPTIONS="--no-deprecation --max-old-space-size=3072"
  - DATABASE_URL= Get value from Coolify -> Projects -> cpm-cms -> mongodb-payload
    - Configuration -> General -> Network -> Mongo URL(public)
    - Reveal, and copy value, paste into .env file as value of DATABASE_URL
    - Change IP address to the IP of the VM
  - PAYLOAD_SECRET= Enter a value at least 25 characters (uppercase letters, lowercase letters, digits)
  - NEXT_PUBLIC_SERVER_URL=http://localhost:3000
  - CRON_SECRET= Enter a value at least 25 characters (uppercase letters, lowercase letters, digits)
  - PREVIEW_SECRET= Enter a value at least 25 characters (uppercase letters, lowercase letters, digits)

---

Continue with [Make some changes in source code](change-source-code.md) for details
