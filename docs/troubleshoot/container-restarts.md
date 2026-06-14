# Payload service keeps restarting

- Coolify builds images successfully, container keeps restarting.
  - From VPS Payload service logs:
  ```bash
  ▲ Next.js 16.2.1
  - Local:         http://localhost:3000
  - Network:       http://0.0.0.0:3000
  ✓ Ready in 0ms
  TypeError: Cannot read properties of undefined (reading 'map')
  at ignore-listed frames
  ```

- Tried lots of things without success
  - Follow [frozen-lockfile](frozen-lockfile.md)
  - Redeploy
  - Forced deploy
- Solved by 
  - Delete mongodb application, payload application, payload project.
  - Delete idle content in VPS
    - [Clean disk space](vps-disk-space.md)
    - Check for package.json, pnpm-lock.yaml on upper directories of project directory. 
      - Check on local development computer
      - Check on VM
      - Check on VPS
      - Move them to a seperate directory, or delete if accidentally created.
  - Start Over
    - [Implement a Payload CMS with Coolify on a Virtual Private Server](payload/publish-payload-cms-vps.md)
  - Sorry, this is not a proper solution, I did not find the real cause. But starting over solved the issue.



