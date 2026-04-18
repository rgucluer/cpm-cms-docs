## Variables:
When you see these variables through the document , enter the values valid for your setup. Do not enter the angle brackets ( < > ).

- General
  - timezone : Universal
    - Utc
    - Europe/Istanbul

- Domain information
  - domain-name: my-domain.com
    - The domain name registered to you. 
    - fqdn.tld
  - dev-domain-name: devserver1.my-domain.com
    - The domain name registered to you. 
    - fqdn.tld
  - servername : serv1
    - Name of a specific server computer

- Host Machine (Developer PC)
  - dev-pc-local-ip: Developer PC local ip address
  - dev-pc-global-ip: Global IP address that Developer PC connects to internet.
  - local-username
  - workspace: workspace
    - The directory to store software source code.
  - workspace-full-path: /home/<local-user-name>/<workspace>
  - coolify-app-directory: coolify-dev
  - coolify-app-full-path: /home/<local-user-name>/<local-workspace>/<coolify-app-directory>
    - example: /home/jack/workspace/coolify-dev
  - payload-app-full-path: /home/<local-user-name>/<local-workspace>/<payload-app-directory>
    - example: /home/jack/workspace/cpm-cms

- GitHub
  - github-user-key: github-ssh-key
  - github-username: githubuser
  - github-name-surname: John Doe
  - github-user-email
  - git-repo-url : https://github.com/<github-username>/cpm-cms
  - github-app-name: 
    - payload-app-123456 ( Used in Virtual Machine )
    - cpm-cms ( Used in Virtual Machine)

- Virtual Machine
  - vm-user-name: vmuser
  - vm-user-key: vmuser-ssh-key
  - vm-ssh-port: 22
  - vm-root-user: root
  - vm-root-key: root-ssh-key
  - virtual-m-ip: IP of Virtual Machine. Lear from the result of multipass list command.
  - vm-server-name : devserver1  
  - vm-coolify-server-name: devserver1

- Virtual Private Server
  - vps-user-name
  - vps-user-key: vps-user-ssh-key
  - vps-ssh-port: 22
  - vps-ip-address:
  - server-name : server1  
  - vps-coolify-server-name: server1
  - generated-app-directory-name: The string that is listed when we list directories in /data/coolify/applications/ used by our application
  - full-application-path-coolify: /data/coolify/applications/<generated-app-directory-name>

- Coolify
  - resource-name : `<github-username>/<github-repo>:<branch>-<a-generated-string>`
  - project-name:
    - cpm-cms
    - payload-app
    - Application Name
      - payload    
  - Traefik ( Coolify Proxy )
    - traefik-user : traefikuser
    - traefik-password
    - traefik-enc-password
    - lego-email : E-mail registered for letsencrypt
    - lego-service-provider-api-token
      - Read your DNS Provider's documentation.
    - lego-service-provider-env-var : HETZNER_API_TOKEN
      - Look up for your DNS Provider's settings in https://go-acme.github.io/lego/dns/#dns-providers
    - lego-service-provider-dns : hetzner
      - Look up for your DNS Provider's settings in https://go-acme.github.io/lego/dns/#dns-providers
    - traefik-ping-address : https://traefik.my-server.com/ping
  - coolify-github-private-ssh-key-name
    - github-app-cpm-cms
  - Sources
    - Source ID
      - Coolify Web UI -> Sources
        - Click the related App Name (cpm-cms)
          - Source ID is the last section of the URL of this web page.
            - https://`<dev-domain-name>`/source/github/`<Source ID>`






