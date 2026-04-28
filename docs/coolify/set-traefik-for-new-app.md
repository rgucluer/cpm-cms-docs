## Set Traefik for the new application

- Deploy payload service

https://coolify.io/docs/knowledge-base/proxy/traefik/wildcard-certs#normal

- get full service name information
    - ssh to VM
    ```bash
    docker ps
    ```

    ```bash
    CONTAINER ID   IMAGE  COMMAND  CREATED  STATUS  PORTS NAMES
    9.........e                                           payload-y....d-18...09
    ```

We use the first two sections of the container name, and adding @docker at the end as payload-y....d@docker

Use payload-y....d@docker in payload-app.yaml in Dynamic Configurations below

- Coolify UI on VM (https://coolify.devserver1.my-domain.com)
  - Servers -> < vm-coolify-server-name > -> Proxy -> Dynamic Configurations
    - +Add
      - Filename: payload-app.yaml
      - Configuration: 
        ```yaml
        http:
          middlewares:
            root-to-www:
              redirectRegex:
                regex: '^((https?:\/\/)|).devserver1.my-domain.com'
                replacement: 'https://www.devserver1.my-domain.com'
                permanent: true
          routers:
            redirect-root:
              rule: Host(`devserver1.my-domain.com`)
              entryPoints:
                - http
                - https
              middlewares:
                - root-to-www
              tls:
                certResolver: letsencrypt
                domains:
                  main: devserver1.my-domain.com
              service: noop@internal
            payload-https:
              rule: Host(`www.devserver1.my-domain.com`)
              entryPoints:
                - https
              tls:
                certResolver: letsencrypt
                domains:
                  main: devserver1.my-domain.com
                  sans:
                    - '*.devserver1.my-domain.com'
              service: payload-.....@docker
        ```

      - Paste the Payload service name as value to service (payload-y....d@docker)
      - Save
      - Restart Proxy
        - Wait a few minutes than close the "Proxy Startup Logs" form

