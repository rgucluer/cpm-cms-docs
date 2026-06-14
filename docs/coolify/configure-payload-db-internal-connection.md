# Use Mongo URL (Internal) for db connection

We use Mongo URL (Internal) in Virtual Private Server. We start using Mongo URL (public) in initial local development. For local development database must be already running in VM/VPS. We connect to the database during local development stage. After deployment of Payload application to VM/VPS we can switch to Mongo URL (Internal). We  can directly use Mongo URL (Internal) in Virtual Private Server. The current document already follows this flow. The documentation below demonstrates the switch from old codebase to current state.

- Leave Mongo URL (public) enabled in Virtual Machine, it is still used in local development.
  - What I mean with local development:
    - As told in [Run application in production mode on local machine](../payload/publish-payload-app.md#run-application-in-production-mode-on-local-machine)
      - `node .next/standalone/server.js`
  
- Disable Mongo URL (public) on VPS

### Remove network related settings from docker-compose.yml

- Edit `< payload-app-full-path >`\docker-compose.yml
  - Remove settings related to network.
  - Remove name

```bash
name: payload-project
networks:
  coolify:
    name: coolify
    external: true
.....
services:
  payload:
    .....
    networks:
      - coolify
```

### Check Payload service name in Dynamic Configuration

- Servers -> < servername > -> Dynamic Configuration -> payload-app.yaml
  - Edit
    - payload-https - service
      - Enter Payload CMS container name's first two segments + @docker as service value
      - Get value from the result of `docker ps` command inside VM/VPS
      - Example: payload-abcdefabcdefabcdefabcdef@docker

### Remove network related settings from Traefik settings

- Coolify UI -> Servers -> < servername > -> Proxy -> Traefik (Coolify Proxy)
- Coolify manages the network, no need to define them again.

```yaml
.....
networks:
  coolify:
    external: true
services:
  traefik:
  .....
  networks:
    - coolify

.....
```

Save, restart proxy. Wait several minutes. 

### Enable Connect To Predefined Network for Payload

- Coolify UI -> Projects -> cpm-cms -> payload -> Configuration -> Advanced
  - Docker Compose
    - Check "Connect to Predefined Network"

---

### Edit docker-compose.yml

- Change build parameters for Payload service

```yaml
services:
  payload:
    container_name: payload
    .....
    pull_policy: build
    image: custom-payload-website:3.84.1
    build:
      context: .
      dockerfile: Dockerfile
      network: host
    .....
    labels:
      traefik.enable: true
      coolify.managed: true
      coolify.proxy: true
volumes:
  node_app:
  node_modules:
```

---


### Use Mongo URL (Internal) for Payload CMS on VPS

- Coolify UI -> Projects -> payload-project production -> Databases: mongodb-payload
  - Mongo URL (Internal)
   - Reveal
   - Click inside textbox
   - Select all
   - Copy

- Coolify UI -> Projects -> payload-project production -> Applications: payload -> Environment Variables
  - DATABASE_URL
    - Reveal, and delete content
    - Paste Mongo URL (Internal) here
      - Check  Available at Buildtime
      - Check  Available at Runtime
      - Click Update

- You can also use Developer view


### Uncheck "Make it publicly available" on VPS

- Coolify UI -> Projects -> payload-project production  -> Databases: mongodb-payload-vps -> Configuration -> General
  - Proxy
    - Uncheck "Make it publicly available"
    - Save

### Forced Deploy Payload CMS

- Coolify UI -> Projects -> cpm-cms -> Applications: payload 
  - Advanced - Force Deploy (without cache)

### References:
- https://docs.docker.com/reference/compose-file/build/
- https://docs.docker.com/reference/compose-file/build/#network
- https://docs.docker.com/reference/compose-file/services
- https://coolify.io/docs/knowledge-base/docker/compose
