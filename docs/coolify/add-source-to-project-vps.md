## Add Source to the Project

- Coolify Web UI -> Projects -> payload-project ( production )
  - Click Resources +New
    - Applications -> Git Based
      - Click Private Repository ( with GitHub App )
        - Select a GitHub App: Click < github-app-name-vps > button
            - Repository: cpm-cms
            - Click Load Repository
            - Branch: main
            - Build Pack: Docker Compose
            - Change Docker Compose Location to:
                - /docker-compose.yml
                - ATTENTION : File extension is .yml not .yaml. Please check & edit.
            - Continue
            - If successful, this loads docker-compose.yml, opens configuration menu.
            - Set Name: payload
              - < Coolify Application Name >
              - Save

### Continue with ... [Configure the Payload Project](configure-payload-vps.md)
