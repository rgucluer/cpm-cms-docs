## Coolify Add a new Source

Following Coolify documentation https://coolify.io/docs/applications/ci-cd/github/setup-app

Visit `https://coolify.< domain-name >`

- Coolify Web UI -> Sources -> Add+
  - New GitHub App
    - Name: < github-app-name >  ( cpm-cms-vps, etc)
    - Continue
    - There are two options:
      - Automated Installation
        - Use this for creating a new GitHub App for your source repository
      - [Manual Installation](#manual-installation)
        - If GitHub App already exists, you can add it to Coolify with Manual Installation.

### Automated Installation

- Automated Installation
  - Selected endpoint
    - Select from select box
      - Select  https://coolify.< my-domain.com >
    - Uncheck `Preview Deployments`
    - Register Now
      - Create GitHub App
        - GitHub App name: < github-app-name > 
        - Click `Create Github App for < github-username >`
        - Confirm access ( GitHub Authentication )        
          - Authenticate
          - Click Install Repositories on GitHub
          - Install `< github-app-name-vps >`
            - Click: Only select repositories
              - Select from "Select repositories"
                - `< my-github-username >`/`< github-repo-name >`
            - Click Install
  - We are back in Coolify UI Sources - GitHub App Form -> 
    - App Name: < github-app-name-vps >
    - Save

#### Continue with [Add Source to the Project](../coolify/add-source-to-project-vps.md)

---

### Manual Installation

- Coolify Web UI -> Sources -> Add+
  - New GitHub App
    - Name: < github-app-name-vps >
      - Example: personal-blog-vps
    - Continue
    - Apply [add-new-source-manual-vps](add-new-source-manual-vps.md)

---

### Continue with [Add Source to the Project](../coolify/add-source-to-project-vps.md)

---

On GitHub Web -> GitHub App
- https://github.com/settings/apps/`< github-app-name >`

Coolify Web UI -> GitHub App
- https://coolify.`< domain-name >`/source/github/`< Source ID >`


