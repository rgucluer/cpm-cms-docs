## Configure the Payload Project

- Coolify Web UI -> Projects -> cpm-cms ( production )
  - Applications -> cpm-cms:dev-...

    - Configuration -> General
      - Name: payload
      - Description: Payload template (website) for Coolify
      - Build Pack: Docker Compose
      - Domains -> Domains for payload:
        - https://www.devserver1.my-domain.com,https://devserver1.my-domain.com
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
      - Docker Compose
        - Connect To Predefined Network : Check

    - Configuration -> Environment Variables
      - Check: Use Docker Build Secrets
      - Production Environment Variables
        - Add the following variables
        - For every variable
          - Check: Available at Buildtime , Check: Available at Runtime
          - Save, Close Form, Update
      - NODE_OPTIONS: --no-deprecation --max-old-space-size=3072
      - HOSTNAME: 0.0.0.0
      - DATABASE_URL: Get value from mongodb-payload Mongo URL (public), and paste as value. 
        - Change IP address to VM IP address ( ....@`IP ADDRESS`/?directConnection=true )
      - PAYLOAD_SECRET: < copy value from local copy .env file to here >
      - NEXT_PUBLIC_SERVER_URL : https://www.devserver1.< domain-name >
      - CRON_SECRET: < Enter_your_password_here >
      - PREVIEW_SECRET: < Enter_your_password_here >
      
    - Configuration -> Git Source
      - Repository:
        - < github-username >/cpm-cms
      - Branch: 
        - dev
      - Commit SHA
        - HEAD
      - Save
 
    - Configuration -> Resource Limits
      - Limit CPUs ( https://docs.docker.com/engine/containers/run/#cpu-share-constraint )
        - Number of CPUs: 0.5
          - Number is a fractional number. 0.000 means no limit.
        - CPU sets to use: Empty
        - CPU Weight: 512
        - Soft Memory Limit: 3g
        - Maximum Memory Limit: 3g
        - Swappiness: 1
        - Maximum Swap Limit: 3g
        - Save
          - Resource limits updated.
        - ( Set Memory values to according to your setup. )

    - Configuration -> General
      - Check & Save

Continue with [Deploy Application](../payload/publish-payload-cms-vps.md#deploy--redeploy-payload-application)

Back to [publish-payload-app.md](../payload/publish-payload-app.md#configure-the-payload-project).