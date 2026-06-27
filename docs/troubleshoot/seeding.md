
- Check ownership of files/directories in payload container

- Last resort:
- Wipe & start over
  - ATTENTION, all data will be deleted.
  - Delete payload service
    - Stop payload service
    - Delete payload service (Danger Zone)
  - Delete database service
    - Stop mongodb-payload service
    - Delete mongodb-payload service (Danger Zone)
  - Recreate database service
    - Apply [payload/create-mongodb.md](../payload/create-mongodb.md)
  - Recreate payload service
    - Add Source to the Project
      - Apply [coolify/add-source-to-project](../coolify/add-source-to-project.md)
        - Project: cpm-cms
        - GitHub App: payload-cms-for-vps
        - Repository: cpm-cms
        - Docker Compose, /docker-compose.yml, dev branch
        - Continue
        - Set Name: personal-blog-dev
        - Save

    - Configure the Payload Project
      - Apply [coolify/configure-payload-dev](../coolify/configure-payload-dev.md)
        - Name: payload
        - Description: Payload template (website) for Coolify
        - Domains for payload: https://www.< dev-domain-name >,https://< dev-domain-name >
        - Different entries from the document
          - NEXT_PUBLIC_SERVER_URL=https://www.< dev-domain-name >
          - DATABASE_URL=< Mongo URL (internal) from Coolify UI >

    - Enter Mongo URL (public) to local .env file DATABASE_URL , save
      - Change IP address to VM IP address ( ....@`IP ADDRESS`/?directConnection=true )

    - Deploy Payload Project
      - VM -> Coolify -> Projects -> < project-name > -> < application-name >
        - Advanced -> Force Deploy

  - Check/Set service name in Traefik Dynamic configuration for payload service
    - service value of payload-https route must be correctly set
      - payload-abc...yz@docker
      - Save

  - Restart Proxy - Traefik
    - VM -> Coolify -> Servers -> < coolify-server-name > -> Restart Proxy

  - Check https://www.< dev-domain-name >
    - Click "Visit the admin dashboard"
    - Create a new Payload admin user
      - Welcome to your dashboard!
        - Click "Seed your database"
          - Seeding with data
            - Message: `Database seeded! You can now visit your website`
            - OK, we are good.
            - Click `visit your website`
            - Homepage renders with images. 
          - Message: "An error ocurred during seeding"
            - Problem continues
            - Hımm, not so good. ( Did not happen in my setup after applying the steps above )

