## Variables:
When you see these variables through the document , enter the values valid for your setup. Do not enter the angle brackets ( < > ).

- Host Machine (Developer PC, Controller PC)
  - local-user-name: System user name
  - workspace: workspace
    - The directory to store software source code.
  - workspace-full-path: /home/< local-user-name >/< workspace >
  - dev-pc-local-ip: Developer PC local IP
  - dev-pc-global-ip: Global IP address that Developer PC connects to internet.

- Domain information
  - domain-name: my-domain.com
    - The domain name registered to you. 
    - fqdn.tld
  - dev-domain-name: devserver1.my-domain.com
  - prod-domain-name: cpm-cms.my-domain.com
  - servername : 
    - Name of a specific server computer
      - server1
      - devserver1
      - devblogserver

- Payload App
  - payload-app-directory: cpm-cms
  - payload-app-full-path: /home/< local-user-name >/< local-workspace >/< payload-app-directory >
    - example: /home/jack/workspace/cpm-cms

- GitHub
  - github-username: githubuser
  - github-repo-name: cpm-cms
  - github-user-key: github-ssh-key
  - github-name-surname: John Doe
  - github-user-email
  - git-repo-url : https://github.com/< github-username >/cpm-cms
  - github-app-name-vm: 
    - cpm-cms ( Used in Virtual Machine)
  - github-app-name-vps: 
    - cpm-cms-vps ( Used in Virtual Private Server)
    - payload-app-vps ( Used in Virtual Private Server)

- Virtual Machine
  - vm-name: coolvm
  - vm-user-name: vmuser
  - vm-user-key: vmuser-ssh-key
  - vm-ssh-port: 22
  - vm-root-user: root
  - vm-root-key: root-ssh-key
  - virtual-m-ip: IP of Virtual Machine. Lear from the result of multipass list command.
  - vm-server-name : devserver1  

- Virtual Private Server
  - vps-user-name
  - vps-user-key: vps-user-ssh-key
  - vps-ssh-port: 22
  - vps-root-key: root-ssh-key
  - vps-ip-address:
  - server-name : server1  
  - vps-coolify-server-name: server1
  - generated-app-directory-name: The string that is listed when we list directories in /data/coolify/applications/ used by our application
  - full-application-path-coolify: /data/coolify/applications/< generated-app-directory-name >

- Coolify
  - resource-name : `< github-username >/< github-repo >:< branch >-< a-generated-string >`
  - vm-coolify-rootkey: SSH key for VM root user
    - my-coolify-root-key
  - coolify-project-name:
    - cpm-cms
    - payload-project
  - coolify-application-name
    - payload
  - coolify-server-name
    - devserver1
    - server1
    - cpm-cms
  - Databases
    - mongodb-payload    
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
            - https://`< dev-domain-name >`/source/github/`< Source ID >`

- General
  - timezone : Universal
    - Utc
    - Europe/Istanbul


Conventions:

If you see a `$` sign in terminal screen, it means content after `$` sign is a command you enter in a terminal and execute.

```bash
$ ls -la
```

Will use to seperate terminal result screens from command lines.





