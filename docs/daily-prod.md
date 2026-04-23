# Daily Operations of Production Environment

## Add ssh keys to ssh agent 
```bash
ssh-agent bash
```

```bash
ssh-add ~/.ssh/< vps-user-key >
```

```bash
ssh-add ~/.ssh/< github-user-key >
```

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

After completing [development](daily-dev.md), let's see production steps

## Merge dev branch to main branch

On Development PC. Switch to main branch
```bash
git switch main
```

Merge dev branch to main branch
```bash
git merge dev
```

## Commit and push your changes to Git repo

```bash
git push origin main
```

## Redeploy on Virtual Private Server
- Coolify Dev UI ( https://coolify.< domain-name > ) 
  - Projects -> < project-name > 
    - Applications -> payload
      - Redeploy

## Git pull dev branch to current state

```bash
git switch dev
```

```bash
git pull origin main
```

```bash
git push origin dev
```

- Repeat

