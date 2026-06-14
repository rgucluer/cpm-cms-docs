# Set Hetzner API Token if you use Hetzner

- Hetzner Console:
  - Create an API token in the Hetzner Console → choose Project → Security → API Tokens.
    - Generate API Token
      - Description: HETZNER_API_TOKEN
      - Permissions: Read&Write
      - Generate API Token
      - Copy and securely store the value somewhere

- Coolify Web User Interface:
  - Keys & Tokens -> Cloud Tokens
    - New Token
      - Provider Hetzner: 
        - Token Name: HETZNER_API_TOKEN
        - API Token: < lego-service-provider-api-key >
        - Validate & Add Token
        - Cloud provider token added successfully.

- It may take some time(several minutes) to "Link to Hetzner Cloud" section to appear. Be patient. 

- Coolify Web User Interface:
  - Servers -> server1 -> Configuration -> General
    - Link to Hetzner Cloud (If available)
      - Hetzner Token
        - Choose HETZNER_API_TOKEN
        - Enter your Hetzner Server ID (Get your ID from Hetzner Console)
          - Click Search by ID
          - If a result appears, click "Link This Server"
            - If not, wait a minute try again.
