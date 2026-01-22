# Publish a Next.js App with Coolify

https://coolify.io/docs/applications/
https://coolify.io/docs/applications/nextjs
https://github.com/coollabsio/coolify-examples/tree/v4.x/nextjs/ssr
https://github.com/vercel/next.js/tree/canary/examples/with-docker-compose
https://coolify.io/docs/builds/packs/docker-compose


## Coolify - Add Resource

- Coolify UI -> Projects -> cpm-cms (production) -> Add Resource
  - Public Repository
    - https://github.com/coollabsio/coolify-examples/tree/v4.x/nextjs/ssr
    - Check Repository
    - Build Pack: Nixpacks
    - Base Directory: /nextjs/ssr
    - Port: 3000
    - Check: Is it a static site?
    - Continue

- Configuration
  - General
    - Name: payload
    - Direction: Allow www & non-www.
    - Domains: https://www.devserver1.my-domain.com
    - Publish Directory: /public
    - Save

- Resource Limits
  - Limit CPUs ( https://docs.docker.com/engine/containers/run/#cpu-share-constraint )
    - Number of CPUs: 0.5
      - Number is a fractional number. 0.000 means no limit.
    - CPU sets to use: Empty
      - CPUs in which to allow execution (0-3, 0,1)
    - CPU Weight: 512
    - Soft Memory Limit: 3g
    - Maximum Memory Limit: 3g
    - Swappiness: 1
    - Maximum Swap Limit: 3g
    - Save
    - ( Set Memory values to according to your setup. )


- Coolify UI -> Projects -> cpm-cms (production) -> payload
  - Deploy/Redeploy
  - Deploy takes time.
    - Click Show Debug Logs
    - Watch for "New container is healthy." message.
    - Check https://www.devserver1.my-domain.com
    - Works, good :) .

Troubleshooting

- "no available server"
  - Check Traefik Dynamic Configuration for https://www.devserver1.my-domain.com

---


Older ...
---

- Edit Traefik Dynamic configuration, and add the app
  - Coolify UI -> Servers -> devserver1 -> Proxy
    - Dynamic Configurations
      - Add settings for the application
```yaml
http:
...
  routers:
    payload-https:
      entryPoints:
        - https
      service: payload
      rule: Host(`www.devserver1.my-domain.com`)
      tls:
        certresolver: letsencrypt
    payload-http:
      middlewares:
        - redirect-to-https
      entryPoints:
        - http
      service: payload
      rule: Host(`www.devserver1.my-domain.com`)
  services:
    payload:
      loadBalancer:
        servers:
          - 'url=https://www.devserver1.my-domain.com'

```
- Save
- Close Edit form
- Restart Proxy
  - This takes some time. Form does not update even after Traefik starts. Wait 30 seconds or 1 minute. Close the form


---
Old ....

## Coolify - Add a new Source
- Coolify Web UI -> Sources -> Add
  - New GitHub App
    - Name: <name-of-github-app>
      - pc-github-app-001
    - Continue
    - Automated Installation
      - Webhook Endpoint
        - Select from select box
          - Select  https://coolify.devserver1.<my-domain.com>
        - Register Now
        - Confirm access ( GitHub Authentication )        
          - Authenticate
        - Create App name
          - GitHub App Name : <name-of-github-app>
          - Click "Create GitHub App for <my-github-username>"
        - Click Install Repositories on GitHub
        - Authorization Request for GitHub - Install <name-of-github-app>
          - Click: Only select repositories
            - Select from "Select repositories"
              - <my-github-username>/<name-of-git-repository>
          - Click Install
  - We are back in Sources -> <my-github-app>

## Add Resource to the Project
- Coolify Web UI -> Projects -> payload-app -> development -> Add Resource
  - Applications -> Git Based
    - Click Private Repo. with GitHub App  
  - Select a GitHub App: Click <name-of-github-app> button
    - Repository: Select your repository
    - Click Load Repository
      - Branch: main
      - Build Pack: Docker Compose
        - Base Directory
          - /
        - Check: Preserve Repository During Deployment
        - Docker Compose Location to:
          - /compose.dev.yaml
        - Continue
  - Newly created resource ( app )
    - Configuration -> General -> Domains -> Domains for <service name>
      - https://www.devserver1.<my-domain.com>  
      - Save.
        - May give DNS validation error:
          - Coolify Web UI -> Settings -> Configuration -> Advanced -> DNS Settings -> DNS Validation: Uncheck
            - Save
          - Coolify Web UI -> Settings -> Configuration -> Advanced -> DNS Settings -> DNS Validation: Check
            - Save
          - Projects -> payload-app -> development -> <github-user>/<name-of-github-app> -> Configuration -> General
            - Save




## Troubleshooting

### Deploy error: Degraded(unhealty)

- Logs
  - [Error: > Couldn't find any `pages` or `app` directory. Please create one under the project root]
    - there is no file in /data/coolify/applications/<app-directory-name>/next-app/public
    - there is no file in /data/coolify/applications/<app-directory-name>/next-app/src
    - src and public directories exists but without any files in them
    - pages directory is in src
    - Why ?
  - Solution: 
    - Coolify Web UI -> Projects -> payload-app -> development -> Configuration -> General -> Build
      - Check: Preserve Repository During Development



