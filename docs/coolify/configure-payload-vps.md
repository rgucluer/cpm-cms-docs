## Configure the Project

- Coolify Web UI -> Projects -> payload-project ( production )
  - Applications -> < github-app-name-vm > ...

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
      - Pre/Post Deployment Commands
        - Post-deployment: `/app/after-deploy.sh`
      - Save
        - Success - Application settings updated!

    - Configuration -> Advanced
      - Build
        - Inject Build Args to Dockerfile: Check
        - Include Source Commit in Build: Uncheck
      - Container:
        - Consistent Container Names: Uncheck
        - Custom Container Name: payload
          - Save
      - Deployment
        - Auto Deploy: Uncheck
        - Preview Deployments: Uncheck
      - Docker Compose
        - Connect To Predefined Network : Check
      - Proxy
        - Force Https: Uncheck
        - Strip Prefixes: Check

    - Configuration -> Environment Variables
      - Check: Use Docker Build Secrets
      - Production Environment Variables
        - Add the following variables
        - For every variable
          - Check: Available at Buildtime , Check: Available at Runtime
          - Save, Close Form, Update
      - NODE_OPTIONS: --no-deprecation --max-old-space-size=2048
      - HOSTNAME: 0.0.0.0
      - DATABASE_URL: Get value from mongodb-payload Mongo URL (internal), and paste as value. 
      - PAYLOAD_SECRET: < copy value from local copy .env file, or enter a different one >
      - NEXT_PUBLIC_SERVER_URL : `https://www.< domain-name >`    
      - CRON_SECRET: < Enter_your_password_here >
      - PREVIEW_SECRET: < Enter_your_password_here >

    - Configuration -> Git Source
      - Repository:
        - < github-username >/cpm-cms
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
      - Check Domains for payload
      - Check & Save

Continue with [Deploy Application](../payload/publish-payload-cms-vps.md#deploy--redeploy-payload-application)

Back to [publish-payload-vps](../payload/publish-payload-cms-vps.md#configure-the-payload-vps).
