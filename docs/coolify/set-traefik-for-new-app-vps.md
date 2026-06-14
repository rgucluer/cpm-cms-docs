## Set Traefik for the new application

- After deploying payload service,
- Get container name information
  - Deployment Logs
    - Projects -> < project-name > -> payload -> Deployments
      - Click to the last successful deployment log
      - Scroll to the end of the log
        - Find "New container started" message
        - Before this row log lists the container name
          ```bash
          Container payload-abcdefghijklmabcdefghi-012345678901 Started
          ```
  - Or use `docker ps` command
    - ssh to VM
    ```bash
    docker ps
    ```

    ```bash
    CONTAINER ID   IMAGE  COMMAND  CREATED  STATUS  PORTS NAMES
    9.........e                                           payload-abcdefghijklmabcdefghi-012345678901
    ```

We use the first two sections of the container name, and adding @docker at the end as payload-abcdefghijklmabcdefghid@docker

Use payload-abcdefghijklmabcdefghi@docker in payload-app.yaml in Dynamic Configurations below

- Coolify UI on VPS (https://coolify.my-domain.com)
  - Servers -> < vps-coolify-server-name > -> Proxy -> Dynamic Configurations
    - +Add/edit
      - Filename: payload-app.yaml
      - Configuration: 
        ```yaml
        http:
          middlewares:
            root-to-www:
              redirectRegex:
                regex: '^((https?:\/\/)|).my-domain.com'
                replacement: 'https://www.my-domain.com'
                permanent: true
          routers:
            redirect-root:
              rule: Host(`my-domain.com`)
              entryPoints:
                - http
                - https
              middlewares:
                - root-to-www
              tls:
                certResolver: letsencrypt
                domains:
                  main: my-domain.com
              service: noop@internal
            payload-https:
              rule: Host(`www.my-domain.com`)
              entryPoints:
                - https
              tls:
                certResolver: letsencrypt
                domains:
                  main: my-domain.com
                  sans:
                    - '*.my-domain.com'
              service: payload-.....@docker
        ```
        
      - Paste the Payload service name + "@docker" as value to service (payload-abcdefghijklmabcdefghid@docker)
        - Use only the first two sections of the container name
          - payload-y....d-123...234
          - payload-y....d@docker
      - Check domain names, change them to your according to your setup.
      - Save
      - Close Form
      - Restart Proxy
        - Wait a few minutes than close the "Proxy Startup Logs" form
          - A white page can appear, click background on edges of white form.
        - Still unresponsive, browse to https://coolify.<domain-name>