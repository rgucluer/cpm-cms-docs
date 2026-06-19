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

- Automated Installation
  - Selected Endpoint
    - Select from select box
      - Select https://coolify.devserver1.< domain-name >
    - Register Now
    - Create Github App for < github-username >
    - Confirm access ( GitHub Authentication )        
      - Authenticate
    - Create App name
      - GitHub App Name : cpm-cms
      - Click "Create GitHub App for < my-github-username >"
    - Click Install Repositories on GitHub
    - Authorization Request for GitHub - Install `< github-app-name-vm >`
      - Click: Only select repositories
        - Select from "Select repositories"
          - `< my-github-username >`/`< git-repo-name >`
      - Click Install
    - We are back in Coolify UI Sources - GitHub App Form -> 
      - App Name: < github-app-name-vm >
      - Save


### Manual Installation

- Coolify Web UI -> Sources -> Add+
  - New GitHub App
    - Name: cpm-cms
    - Continue
    - Manual Installation
      - Continue
      - Apply [add-new-source-manual](add-new-source-manual.md)

---

### Continue with [Add Source to the Project](add-source-to-project.md)
 
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
