# Coolify - Add a new Source

On GitHub Web -> GitHub App
- https://github.com/settings/apps/`< github-app-name >`

Coolify Web UI -> GitHub App
- https://coolify.`< dev-domain-name >`/source/github/`< Source ID >`


## Coolify - Add a new Source

Following Coolify documentation https://coolify.io/docs/applications/ci-cd/github/setup-app

Visit `https://coolify.< dev-domain-name >`

- Coolify Web UI -> Sources -> Add+
  - New GitHub App
    - Name: < github-app-name >
    - Continue
    - There are two options:
      - Automated Installation
        - Use this for creating a new GitHub App for your source repository
      - [Manual Installation](#manual-installation)
        - If GitHub App already exists, you can add it with Manual Installation.

### Automated Installation

- Automated Installation
  - Webhook Endpoint
    - Select from select box
      - Select  https://coolify.devserver1.< my-domain.com >
    - Register Now
    - Confirm access ( GitHub Authentication )        
      - Authenticate
    - Create App name
      - GitHub App Name : cpm-cms
      - Click "Create GitHub App for < my-github-username >"
- Click Install Repositories on GitHub
- Authorization Request for GitHub - Install cpm-cms
  - Click: Only select repositories
    - Select from "Select repositories"
      - < my-github-username >/cpm-cms
  - Click Install
- We are back in Sources -> < my-github-app >
  - Save


### Manual Installation

#### How to get GitHub App information

- Open your GitHub Account
  - Open your private git repository
  - Settings
    - Integrations -> GitHub Apps (Bottom of Left Menu)
      - cpm-cms -> Configure ( Hover over button and get the link)
        - Right most of the link is Installation ID
        - https://github.com/settings/installations/< installation_id >
      - Click cpm-cms -> Configure 
        - Confirm access: Authenticate, and continue
      - Click App Settings (Right of cpm-cms title )
        - This is application settings page (GitHub Web - GitHub App Settings), We can get/set information in this page, and fill the Coolify form.

- On GitHub Web - GitHub App Settings
  - Permissions & Events
    - Repository permissions
      - Administration: Read-only
      - Contents: Read-only
      - Metadata: (Mandatory) Read-only
      - Pull requests: Read & Write
    - Account permissions
      - Email addresses: Read-only
  - Subscribe to events
    - Check: Pull request
    - Check: Push
  - Save changes

- On GitHub Web - GitHub App Settings - General
  - Client secrets
    - If you have the current Client secret at hand, ready it for later use
    - If not , Click Generate a new client secret and save the secret for later use.

- On GitHub Web - GitHub App Settings - General
  - Private Keys
    - If you have the current Private Key at hand, ready it for later use
    - If not , Click Generate a private and save the secret for later use.

- Coolify Web UI
  - Add the related private key to Coolify Keys & Tokens
  - Keys & Tokens
    - Private Keys -> +Add
      - New Private Key Form
        - Name: github-app-cpm-cms
        - Private Key: Copy content of the private key(.pem file) on previous step and paste it here.
        - Form will fill the Public Key value itself. 
        - Save

- Coolify Web UI -> Sources -> Add+
  - New GitHub App
    - Name: cpm-cms
    - Continue
    - Manual Installation
      - Continue

- Coolify Web UI -> Sources -> GitHub App :
  - Take note of Source ID
    - Step 6 of Manual Installation: 
      - https://coolify.io/docs/applications/ci-cd/github/setup-app#manual-installation
      - The last section of the Coolify GitHub App Page URL
        - https://coolify.`< dev-domain-name >`/source/github/`< Source ID >`

- On GitHub Web - GitHub App Settings - General
  - Set Post installation Setup URL
    - Edit URL, paste Source ID as last part of the URL
    - https://`< dev-domain-name >`/webhooks/source/github/install?source=`< Source ID >`

- Coolify Web UI -> Sources -> GitHub App Page
  - App Name: cpm-cms
  - HTML Url: https://github.com
  - API Url: https://api.github.com
  - User: git
  - Port: 22
  - App Id: `< Get information from GitHub App Page >`
  - Installation Id: 
    - Installation Id: It is the right most part of GitHub App configure button link
      - https://github.com/settings/installations
      - Hoover over configure button, Installation Id is the last section of the link
  - Client Id: `< Get information from GitHub App Page >`
  - Client Secret: `< Enter GitHub Client Secret here >`
    - You must save Client Secret while creating it. If you do not know the Client Secret, you can create a new one(On GitHub), and enter the new Client Secret to Coolify.
  - Webhook Secret: `< Enter GitHub Webhook Secret here >`
    - If you do not know the Webhook Secret, you can change the secret. But in that case you must update existing integrations.
  - Save
  - Set Private Key
    - You must add the private key created on GitHub to Coolify Keys && Tokens section first, then select this key . 
    - Private Key: Select from list: `< coolify-github-private-ssh-key-name >`
      - github-app-cpm-cms
      - If you can not see the key in select box, refresh the page
  - Save
  - Click Sync Name
    - "GitHub App name and SSH key name synchronized successfully."

### Continue with [Add Source to the Project](../coolify/add-source-to-project.md)
 
---

## Errors

- Failed to fetch GitHub App information: Integration not found
  - Fill in App Id, and Installation Id
- Failed to fetch GitHub App information: A JSON web token could not be decoded

---

## References

- https://coolify.io/docs/applications/ci-cd/github/setup-app
