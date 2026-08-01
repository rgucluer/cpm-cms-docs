## Coolify - Add a new Source

Following Coolify documentation https://coolify.io/docs/applications/ci-cd/github/setup-app

Visit `https://coolify.< dev-domain-name >`

- Coolify Web UI -> Sources -> Add+
  - New GitHub App
    - Name: < github-app-name-vm >
    - Continue
    - There are two options:
      - Automated Installation
        - Use this for creating a new GitHub App for your source repository
      - [Manual Installation](#manual-installation)
        - If GitHub App already exists, you can add it to Coolify with Manual Installation.

### Automated Installation

- Selected Endpoint
  - Select from select box
    - Select https://coolify.< dev-domain-name >
    - Uncheck `Preview Deployments`
  - Register Now
  - Create Github App:
    - GitHub App Name : cpm-cms-vm
    - Click Create Github App for < github-username >
  - Allow browser if browser wants permission for this operation
  - Click Install Repositories on GitHub
  - Install cpm-cms:
    - Authorization Request for GitHub - Install `< github-app-name-vm >`
      - Click: Only select repositories
        - Select from "Select repositories"
          - `< github-username >`/`< github-repo-name >`
      - Click Install
  - We are back in Coolify UI Sources - GitHub App Form 
    - App Name: < github-app-name-vm >
    - Save

#### Continue with : Add Source to the Project [coolify/add-source-to-project](../payload/publish-payload-app.md#add-source-to-the-project-coolifyadd-source-to-project)

---

### Manual Installation

- Coolify Web UI -> Sources -> Add+
  - New GitHub App
    - Name: cpm-cms
    - Continue
    - Manual Installation
      - Continue
      - Apply [add-new-source-manual](add-new-source-manual.md)

---

## Continue with : Add Source to the Project [coolify/add-source-to-project.md](../payload/publish-payload-app.md#add-source-to-the-project-coolifyadd-source-to-project)

---

## Errors

- Failed to fetch GitHub App information: Integration not found
  - Fill in App Id, and Installation Id

---

On GitHub Web -> GitHub App
- https://github.com/settings/apps/`< github-app-name >`

Coolify Web UI -> GitHub App
- https://coolify.`< dev-domain-name >`/source/github/`< Source ID >`

---

## References

- https://coolify.io/docs/applications/ci-cd/github/setup-app
