# Create a MongoDB using Coolify

- Coolify UI -> Projects -> cpm-cms (production) 
  - Resources
    - +Add Resource (or +New )
    - Databases
      - Mongo DB 
          - Configuration:
            - Name: mongodb-payload
            - Proxy
              - Public Port: 27017
            - Save
          - Resource Limits
            - Number of CPUs: 0.25
            - CPU Weight: 256
            - Limit Memory:
              - Soft Memory Limit: 1g
              - Swappiness: 1
              - Maximum Memory Limit: 1g
              - Maximum Swap Limit: 1g
            - Save
        - Start/Restart
          - Close "Database Startup" form after "Database started." message
          - Check for green Running (healthy) label
          - Save
        - Coolify UI -> Projects -> cpm-cms (production) 
          - mongodb-payload
            - Configuration - General
              - Check : Proxy: Make it publicly available
                - Wait
              - Save
              - We will use Mongo URL (public) in later steps.

Continue with [Install nvm, and Node](../nextjs/install-nvm-node.md)

Optional:

## Install MongoDB Compass (Debian)
https://www.mongodb.com/docs/compass/install/?operating-system=linux&package-type=.deb

```bash
wget https://downloads.mongodb.com/compass/mongodb-compass_1.49.6_amd64.deb
```

```bash
sudo apt install ./mongodb-compass_1.49.6_amd64.deb
```

```bash
mongodb-compass
```

### Create new connection
- Start MongoDB service in Coolify
- Click "Add new connection"
- URI: Paste Mongo URL (public) here
  - Delete IP address ,and enter IP address of Virtual Machine
  - Save & Connect
