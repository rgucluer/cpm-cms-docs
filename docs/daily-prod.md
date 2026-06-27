# Daily Operations of Production Environment

## Add ssh keys to ssh agent 
```bash
ssh-agent bash
```

```bash
ssh-add ~/.ssh/< vps-user-key >
```

```bash
ssh-add ~/.ssh/coolifyrootkey
```

```bash
ssh-add ~/.ssh/< github-user-key >
```

Open a second terminal, do those above. Use one to ssh to VM/VPS, the other for local operations.

## ssh to VPS
```bash
ssh root@< vps-ip-address > -p < ssh-port >
```

## Update apt packages
On VPS

```bash
apt update
```

If there are packages to upgrade:
```bash
apt upgrade
```

Disconnect

After completing [development](daily-dev.md), let's see production steps

## On Development PC: Merge dev branch to main branch

On Development PC. Switch to main branch
```bash
git checkout main
```

Merge dev branch to main branch
```bash
git merge --squash dev
```

## Commit and push your changes to Git repo

```bash
git commit
```

```bash
git push origin main
```

## Stop & Deploy on Virtual Private Server
- !ATTENTION! Deploy operation will delete all content. 
- TODO: Implement Backup/Restore operations before/after deployment.
- Coolify UI on VPS ( https://coolify.< domain-name > ) 
  - Projects -> < project-name > 
    - Applications -> payload
      - Stop -> Continue -> Confirm
        - Wait for `Exited` label
      - Deploy
        - Check the green label above stays green "Running(healthy) for a couple of ten seconds...
      - After deployment, node will build Payload CMS. It takes time (3 - 4 minutes).
      - Open https://www.< dev-domain-name >/admin
        - Login
        - Click Seed your database
          - We do this after a deployment
          - TODO: Implement a backup/restore process to persist existing content between deployments.
          - Open https://www.< dev-domain-name >
          - Home page renders with images and default theme


## Sync dev branch to current state of main branch

Delete already merged local dev branch
```bash
git branch -d dev
```

To force delete we can use -D option

Create dev branch

```bash
git switch -c dev
```

We can continue to daily-dev



