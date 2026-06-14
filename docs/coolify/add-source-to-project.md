## Add Source to the Project

- Coolify Web UI -> Projects -> cpm-cms (production)
  - Click Resources +New
    - Applications -> Git Based
      - Click Private Repository ( with GitHub App )
        - Select a GitHub App: Click cpm-cms button
          - Repository: cpm-cms
          - Click Load Repository
            - Branch: dev
            - Build Pack: Docker Compose
            - Change Docker Compose Location to:
              - /docker-compose.yml
              - ATTENTION : File extension is .yml not .yaml. Please check & edit.
            - Continue
            - If successful, this loads docker-compose.yml, opens configuration menu.

### Continue with ... [Configure the Payload Project](configure-payload-dev.md)
