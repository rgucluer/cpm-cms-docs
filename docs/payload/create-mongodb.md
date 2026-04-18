# Create a MongoDB using Coolify

- Coolify UI -> Projects -> cpm-cms (production) 
  - Resources
    - +Add Resource (or +New )
    - Databases
      - Mongo DB 
          - Configuration:
            - Name: mongodb-payload
              - Name keeps refreshing. Copy "mongodb-payload", paste into Name input, press Save.
            - Proxy
              - Public Port: 27017
                - Same problem here, it keeps refreshing.
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
        - Coolify UI -> Projects -> cpm-cms (production) 
          - mongodb-payload
            - Configuration - General
              - Check : Proxy: Make it publicly available
                - Wait
              - Save
              - We will use Mongo URL (public) in later steps.

Continue with [Create a Payload CMS Application](create-payload-cms.md)


Optional:

## Install MongoDB Compass

On Developer PC
```bash
paru -S mongodb-compass
```

```bash
paru -S simdjson
```

### Create new connection
- Start MongoDB service in Coolify
- Click "Add new connection"
- URI: Paste Mongo URL (public) here
  - Delete IP address ,and enter IP address of Virtual Machine
  - Save & Connect
