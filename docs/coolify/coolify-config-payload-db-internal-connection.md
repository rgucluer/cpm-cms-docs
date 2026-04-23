# Use Mongo URL (Internal) for db connection

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
  - Network
    - Check "Connect to Predefined Network"

---

Convert Database public connection to internal connection

### Use Mongo URL (Internal) for Payload CMS


## Do not implement following ... 

### Old one - Use Mongo URL (Internal) for Payload CMS

- Coolify distinguishes between Runtime and Build-time environment variables.
- Use Mongo URL (Public) in build context
  - Coolify Docker build helper container can not connect to database during docker image build, they are isolated and in different network. We use Mongo URL (public) for connection during build time

-  Bad news, I can not define same environment variable seperately for build and run time. I can define one variable and select when it is used. So I have to manually change DATABASE_URL before deploy, and change it again after deploy. Not so convenient.
  - Coolify UI -> Projects -> cpm-cms production -> payload -> Environment Variables
    - DATABASE_URL
      - Reveal, and delete content
      - Enter Mongo URL (public) here
        - Edit end enter local IP of Virtual Machine (multipass list)
        - Leave it as is for VPS
        - Check   Available at Buildtime
        - Uncheck Available at Runtime
        - Update
  - After Deploy, repeat for Mongo URL (Internal)
  - Coolify UI -> Projects -> cpm-cms production -> payload -> Environment Variables
    - DATABASE_URL
      - Reveal, and delete content
      - Enter Mongo URL (Internal) here
        - Edit end enter local IP of Virtual Machine (multipass list)
        - Leave it as is for VPS
        - Uncheck   Available at Buildtime
        - Check     Available at Runtime
        - Update

- Coolify UI -> Projects -> cpm-cms -> Applications: payload
  - Configuration - Environment Variables
    - DATABASE_URL
      - Reveal , select all textbox, delete
      - Paste 
      - Click Update

- You can also use Developer view


### Uncheck "Make it publicly available"

- Coolify UI -> Projects -> cpm-cms -> mongodb-payload -> Configuration -> General
  - Proxy
    - Uncheck "Make it publicly available"




### Forced Deploy Payload CMS

- Coolify UI -> Projects -> cpm-cms -> Configuration 
  - Anvanced - Force Deploy (without cache)


Payload Deployment Error:

```bash
[23:56:22] ERROR: Error: cannot connect to MongoDB. Details: getaddrinfo ENOTFOUND container_name
```

Current  DATABASE_URL is
mongodb://root:root_password@< container-name >/?directConnection=true

Gemini suggests
mongodb://root:root_password@< container-name >:27017/payload?authSource=admin

Change DATABASE_URL, Update, Advance Forced Deploy
ERROR: Error: cannot connect to MongoDB. Details: getaddrinfo ENOTFOUND < container-name >

Did not work:
- Use mongodb-payload instead of contaner name in DATABASE_URL 

- Coolify  how to connect to mongodb in the same project ?

Docker Compose build 
When Compose is confronted with both a build subsection for a service and an image attribute, it follows the rules defined by the pull_policy attribute.

entitlements defines extra privileged entitlements to be allowed during the build.

### References:
- https://docs.docker.com/reference/compose-file/build/
- https://docs.docker.com/reference/compose-file/build/#network
- https://docs.docker.com/reference/compose-file/services
- https://coolify.io/docs/knowledge-base/docker/compose
