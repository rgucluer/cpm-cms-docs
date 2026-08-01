## Set Traefik for the new application

- After deploying payload service,
- Get Payload application service name information
  - Coolify UI -> Projects : cpm-cms -> Applications: payload
    - Configuration -> General
    - Click Details
      - Service Name : Resource Name + "-" + Resource UUID
      - Service Name : payload-some25charsuuidstring1234

Use payload-some25charsuuidstring1234@docker in payload.yaml in Dynamic Configurations below

- Coolify UI on VM (https://coolify.< dev-domain-name >)
  - Servers -> < vm-server-name > -> Proxy -> Dynamic Configurations
    - +Add/Edit
      - Filename: payload.yaml
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
              service: payload-some25charsuuidstring1234@docker
        ```
      - Paste the Payload service name payload-some25charsuuidstring1234@docker as value to service ( < coolify-application-name >-< payload-container-uuid >@docker )
      - Check spaces, do not use tabs, use only spaces. Indentation is important.
      - Save
      - Close Form
      - Restart Proxy
        - Wait a few minutes than close the "Proxy Startup Logs" form
          - A white page can appear, click background on edges of white form.
        - Still unresponsive, browse to https://coolify.<domain-name>

## Continue with : Check the Application [check-the-application](../payload/publish-payload-app.md#check-the-application)

