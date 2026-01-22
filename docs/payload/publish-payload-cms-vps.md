# Publish a Payload CMS on a Virtual Private Server

We will use the Payload Website template, https://github.com/payloadcms/payload/tree/main/templates/website .

https://payloadcms.com/docs/getting-started/what-is-payload

https://payloadcms.com/docs/production/deployment

## On VPS
```bash
corepack use pnpm@latest-10
```

## Add a new Project

- Coolify UI -> Projects
  - Click `+Add`
    - New Project
      - Name: payload-vps
      - Description: Payload website template for Coolify
      - Continue

## Create a MongoDB using Coolify

- Coolify UI -> Projects -> payload-vps (production) 
  - Resources Add Resource (or +New )
  - Databases
    - Mongo DB 
      - Configuration:
        - Name: mongodb-payload-vps
        - Proxy
          - Public Port: 27017
        - Save
      - Resource Limits
        - Number of CPUs: 0.25
        - CPU Weight: 256
        - Limit Memory:
          - Soft Memory Limit: 1g
          - Maximum Memory Limit: 1g
          - Swappiness: 1
          - Maximum Swap Limit: 1g
        - Save
      - Start/Restart
        - Close "Database Startup" form after "Database started." message
        - Check for green Running (healthy) label
      - Coolify UI -> Projects -> payload-vps (production) 
        - mongodb-payload-vps 
          - Configuration - General
            - Check : Proxy: Make it publicly available
            - Wait
              - We will use Mongo URL in later steps.

## Create a local copy of Payload CMS Website template

We created the files during development stage. Merge development to main branch.

```bash
cd <workspace-full-path>/payload-app
```

https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging

```bash
git checkout main
```

```bash
git merge dev
```

```bash
git push origin main
```

## Open Coolify Web UI

Visit `https://coolify.<domain-name>`

## Coolify - Add a new Source

- Coolify Web UI -> Sources -> Add+
  - New GitHub App
    - Name: payload-app-260107
    - Continue
    - Automated Installation
      - Webhook Endpoint
        - Select from select box
          - Select  https://coolify.<my-domain.com>
        - Register Now
        - Confirm access ( GitHub Authentication )        
          - Authenticate
        - Create App name
          - GitHub App Name : payload-app-260107
          - Click "Create GitHub App for <my-github-username>"
    - Click Install Repositories on GitHub
    - Authorization Request for GitHub - Install payload-app-260107
      - Click: Only select repositories
        - Select from "Select repositories"
          - <my-github-username>/cpm-cms
      - Click Install
    - We are back in Coolify UI - GitHub App Form
      - Save

## Add Source to the Project
- Coolify Web UI -> Projects -> payload-vps ( production )
  - Click Resources +New
    - Applications -> Git Based
      - Click Private Repo. with GitHub App
        - Create a new Application
          - Click : +Add GithubApp
            - Select a GitHub App: Click payload-app-260107
              - Repository: cpm-cms
              - Click Load Repository
                - Branch: main
                - Build Pack: Docker Compose
                - Change Docker Compose Location to:
                  - /docker-compose.yml
                  - ATTENTION : File extension is .yml not .yaml. Please check & edit.
                - Continue
                - If successful, this loads docker-compose.yml, opens configuration menu.

## Configure the Project
- Coolify Web UI -> Projects -> payload-vps ( production )
  - Applications -> <github-username>:main-...
    - Configuration -> General
      - Name: payload
      - Description: Payload template (website) for Coolify
      - Build Pack: Docker Compose
      - Domains -> Domains for payload:
        - https://my-domain.com,https://www.my-domain.com
      - Build
        - Check: Preserve Repository During Deployment
      - Docker Compose Location : /docker-compose.yml
      - Docker Compose
        - Check : Escape special characters in labels
      - Save

  - Configuration -> Advanced
    - Auto Deploy: Uncheck
    - Inject Build Args to Dockerfile: Check
    - Include Source Commit in Build: Uncheck
    - Force Https: Check
    - Strip Prefixes: Check
    - Container Names:
      - Consistent Container Names: Uncheck
      - Custom Container Name: payload
        - Save
    - Network - Connect To Predefined Network : Check

  - Configuration -> Environment Variables
    - Check: Use Docker Build Secrets
    - Production Environment Variables
      - Add the following variables
    - APP_ROOT_DN: my-domain.com
      - Check: Available at Buildtime , Check: Available at Runtime
      - Save
    - DATABASE_URL: Get value from mongodb-payload-vps (public), and paste here.
        - Check: Available at Buildtime , Check: Available at Runtime
        - Save, Close Form, Update
    - HOSTNAME: 0.0.0.0
      - Check: Available at Buildtime , Check: Available at Runtime
      - Save, Close Form, Update
    - NEXT_PUBLIC_SERVER_URL : `https://www.<domain-name>`
      - Check: Available at Buildtime , Check: Available at Runtime
      - Save, Close form, Update
    - NODE_OPTIONS: --no-deprecation --max-old-space-size=3072
      - Check: Available at Buildtime , Check: Available at Runtime
      - Save, Close form, Update
    - PAYLOAD_SECRET: <copy value from local copy .env file >
      - Check: Available at Buildtime , Check: Available at Runtime
      - Save, Close form, Update
  - Configuration -> Git Source
    - Repository:
      - <github-username>/cpm-cms
    - Branch: 
      - main
      - Save, Close form

  - Coolify Web UI -> Projects -> payload-vps ( production )
    - Resource Limits
      - Limit CPUs ( https://docs.docker.com/engine/containers/run/#cpu-share-constraint )
        - Number of CPUs: 0.5
          - Number is a fractional number. 0.000 means no limit.
        - CPU sets to use: Empty
        - CPU Weight: 512
        - Soft Memory Limit: 2g
        - Maximum Memory Limit: 2g
        - Swappiness: 1
        - Maximum Swap Limit: 2g
        - Save
        - ( Set Memory values to according to your setup. )

  - Coolify UI -> Servers -> server1 -> Proxy -> Dynamic Configurations
    - payload-app.yaml
    ```yaml
    http:
      middlewares:
        root-to-www:
          redirectRegex:
            regex: '^((https?:\/\/)|).my-domain.com'
            replacement: 'https://www.my-domain.com'
            permanent: true
      routers:
        redirect-root:
          rule: Host(`my-domain.com`)
          entryPoints:
            - http
            - https
          middlewares:
            - root-to-www
          tls:
            certResolver: letsencrypt
            domains:
              main: my-domain.com
          service: noop@internal
        payload-https:
          rule: Host(`www.my-domain.com`)
          entryPoints:
            - https
          tls:
            certResolver: letsencrypt
            domains:
              main: my-domain.com
              sans:
                - '*.my-domain.com'
          service: payload@docker
    ```
    - Save

  - Coolify UI -> Projects -> payload-vps (production) -> payload
    - Deploy/Redeploy
    - Deploy takes time.
      - Click Show Debug Logs
      - Watch for "New container started." message.
      - Wait for a couple of minutes, watch Running label. Check it stays "Running (healthy)".

  - Coolify UI -> <server-name> -> Restart Proxy
    - Confirm ... -> Restart Proxy

## Change service name in Traefik dynamic configuration
- Coolify UI -> Servers -> server1 -> Proxy -> Dynamic Configurations
  - Edit payload-app.yaml
  - http -> routers -> payload-https:
    - Paste/enter the payload service name we copied.
    - Save

## Check the Application
- Check `https://www.<domain-name>`
  - Works, good `:)` .

- Login ( https://www.<domain-name>/admin )
  - Click "Seed your database", wait ...

- Visit `https://www.<domain-name>`

- Learn more ... [https://payloadcms.com/docs/getting-started/what-is-payload](https://payloadcms.com/docs/getting-started/what-is-payload)

---

## Did not worked - Enable TLS for MongoDB
- Coolify Web UI -> Projects -> payload-vps ( production ) -> payload -> stop (Check clean)
- Coolify Web UI -> Projects -> payload-vps ( production ) -> mongodb-payload-vps -> stop (clean)
  - Wait
  - Refresh page
  - Check: SSL Configuration - Enable SSL
    - require (secure)
  - Save
  - SSL Configuration
    - Click - Regenerate SSL Certificates
      - Continue
      - Restart
        - Restart Database
        - Close form after "Database started" message.
- Coolify Web UI -> Projects -> payload-vps ( production ) -> mongodb-payload-vps 
  - Terminal
    - ```bash
      chown mongodb:mongodb /etc/mongo/certs/ca.pem
      ```   
- Coolify Web UI -> Projects -> payload-vps ( production ) -> mongodb-payload-vps
  - Reveal - Mongo URL (public)
  - Copy content of Mongo URL (public)
- Coolify Web UI -> Projects -> payload -> Configuration ->  Environment Variables
  - Paste into value area of DATABASE_URL
  - Update

- Coolify Web UI -> Projects -> payload-vps ( production ) -> payload

- Deploy


---

### Redirect did not work
Create Redirect from non-www to www URL
- Payload Admin UI
  - Redirects
    - Create new Redirect
      - From URL: https://my-domain.com
      - To URL Type : Custom URL
        - Custom URL: https://www.my-domain.com
      - Save

### Did not worked - Use Mongo URL (internal) URL for database connection

- Coolify UI -> Projects -> payload-vps (production)
  - payload
    - Environment Variables
      - DATABASE_URL
        - mongodb://root:<root_password>@<container_name>:27017/?directConnection=true
    - Redeploy

- Did not work
  - DATABASE_URL : mongodb://mongodb-payload-vps:27017/payload

- Coolify UI -> Projects -> payload-vps (production) -> mongodb-payload-vps
  - Configuration -> General -> Network
    - Port Mappings: 27017:27017
    - Mongo URL (internal)
    - Restart -> Restart Database
- Deploy/Redeploy Payload App

---

## Error: cannot connect to MongoDB. 
- Details: getaddrinfo ENOTFOUND <container_name>

---

## Error: cannot connect to MongoDB. Details: ENOENT: no such file or directory, open '/etc/mongo/certs/ca.pem'

---

## Troubleshooting

### Error after checking "Make it publicly available"
Bind for 0.0.0.0:27017 failed: port is already allocated

- Coolify UI -> Projects -> payload-vps (production)
  - Configuration -> Traefik (Coolify Proxy)
    - Remove 27017 port from ports
    - Save
  - Restart Proxy

### Error: #1 [internal] load local bake definitions

```bash
sudo systemctl restart docker
```

Try deploying again.

## References
- https://github.com/coollabsio/coolify-examples/tree/v4.x
- https://github.com/coollabsio/coolify-examples/tree/v4.x/nextjs/ssr 
- https://github.com/coollabsio/coolify-examples/tree/v4.x/nextjs/spa
- https://coolify.io/self-hosted/


Older

---

## Use TLS with MongoDB Connection

- Stop MongoDB Service
  - Coolify UI -> Projects -> payload-vps (production) -> mongodb-payload-vps
    - Stop
    - Uncheck : Run Docker Cleanup (...)
      - Confirm ... -> Confirm    
    - Refresh page
- SSL Configuration
  - Check : Enable SSL
  - SSL Mode: prefer (secure)
  - Save
  - Start
- Copy Reveal Mongo URL (public)
  - Reveal Mongo URL (public) 
  - Select All
  - Copy
- Change DATABASE_URL in Payload
  - Coolify UI -> Projects -> payload-vps (production) -> payload
    - Environment Variables
      - DATABASE_URL
        - Reveal
        - Clear value area
        - Paste into value area
        - Update
        - Redeploy

## References
- https://coolify.io/docs/knowledge-base/docker/compose#connect-to-predefined-networks
- https://coolify.io/docs/knowledge-base/proxy/traefik/redirects
- https://doc.traefik.io/traefik/reference/routing-configuration/http/middlewares/redirectregex/
- https://payloadcms.com/docs/production/deployment


