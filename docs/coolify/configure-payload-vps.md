## Configure the Project

- Coolify Web UI -> Projects -> payload-vps ( production )
  - Applications -> <github-app-name>:main-...

    - Configuration -> General
      - Name: payload
      - Description: Payload template (website) for Coolify
      - Build Pack: Docker Compose
      - Domains -> Domains for payload:
        - https://www.my-domain.com,https://my-domain.com
        - Save
      - Build
        - Base Directory: /
        - Check: Preserve Repository During Deployment
        - Docker Compose Location : /docker-compose.yml
      - Docker Compose
        - Check : Escape special characters in labels
      - Save
        - Success - Application settings updated!

    - Configuration -> Advanced
      - Auto Deploy: Uncheck
      - Inject Build Args to Dockerfile: Check
      - Include Source Commit in Build: Uncheck
      - Force Https: Uncheck
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
        - For every variable
          - Check: Available at Buildtime , Check: Available at Runtime
          - Save, Close Form, Update
      - NODE_OPTIONS: --no-deprecation --max-old-space-size=2048
      - HOSTNAME: 0.0.0.0
      - DATABASE_URL: Get value from mongodb-payload-vps (public), and paste here.
        - Change IP address to VPS public IP address ( ....@`IP ADDRESS`/?directConnection=true )
      - PAYLOAD_SECRET: <copy value from local copy .env file >
      - NEXT_PUBLIC_SERVER_URL : `https://www.<domain-name>`    
      - CRON_SECRET: <Enter_your_password_here>
      - PREVIEW_SECRET: <Enter_your_password_here>

      - Configuration -> Git Source
        - Repository:
          - <github-username>/cpm-cms
        - Branch: 
          - main
        - Commit SHA
          - HEAD
        - Save


    - Configuration -> Resource Limits
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

    - Configuration -> General
      - Check & Save

Continue with [Set Traefik for the new application](./set-traefik-for-new-app-vps.md)

Back to [publish-payload-vps](../payload/publish-payload-cms-vps.md#configure-the-payload-project).