## Add Source to the Project

- Coolify Web UI -> Projects -> payload-project ( production )
  - Click Resources +New
    - Applications -> Git Based
      - Click Private Repository ( with GitHub App )
        - Select a GitHub App: Click < github-app-name-vps > button
            - Repository: < github-repo-name >
              - cpm-cms
            - Click Load Repository
            - Branch: main
            - Build Pack: Docker Compose
            - Change Docker Compose Location to:
                - /docker-compose.yml
                - ATTENTION : File extension is .yml not .yaml. Please check & edit.
            - Continue
            - If successful, this loads docker-compose.yml, opens configuration menu.
            - Set Name: 
              - < coolify-application-name >
            - Save

### Continue with : Configure the Payload Project [coolify/configure-payload-vps](../payload/publish-payload-cms-vps.md#configure-the-payload-project-vps)

