# Install & run Coolify on a Virtual Machine

Following [self hosted installation instructions](https://coolify.io/self-hosted/) for [Coolify](https://coolify.io/)

https://coolify.io/docs/get-started/installation#quick-installation-recommended

ssh to the virtual machine
```bash
ssh root@< virtual-m-ip >
```

```bash
nano /etc/ssh/sshd_config
```
Ensure these settings are present:
```bash
PermitRootLogin prohibit-password
PubkeyAuthentication yes
PasswordAuthentication no
```
Save & Exit

Restart SSH Service
```bash
systemctl restart ssh
```

Update system packages, and restart if necassary.

Run Installation Command

```bash
cd ~
```

```bash
curl -fsSL https://cdn.coollabs.io/coolify/install.sh | bash
```

Result
```bash
   ____                            _         _       _   _                 _
  / ___|___  _ __   __ _ _ __ __ _| |_ _   _| | __ _| |_(_) ___  _ __  ___| |
 | |   / _ \| '_ \ / _` | '__/ _` | __| | | | |/ _` | __| |/ _ \| '_ \/ __| |
 | |__| (_) | | | | (_| | | | (_| | |_| |_| | | (_| | |_| | (_) | | | \__ \_|
  \____\___/|_| |_|\__, |_|  \__,_|\__|\__,_|_|\__,_|\__|_|\___/|_| |_|___(_)
                   |___/


Your instance is ready to use!

You can access Coolify through your Public IPV4: http://< your-public-ip >:8000

If your Public IP is not accessible, you can use the following Private IPs:

http://10.0.0.1:8000
http://10.0.1.1:8000
< another-ip-here >

WARNING: It is highly recommended to backup your Environment variables file (/data/coolify/source/.env) to a safe location, outside of this server (e.g. into a Password Manager).
```

- Backup your /data/coolify/source/.env file on VM to a safe place
- Open a web browser, and browse to `http://< virtual-m-ip >:8000`
  - Fill in the Coolify "Create an account" form, and register your Coolify root user.

- After registration we see
  - Welcome to Coolify - Connect your first server and start deploying in minutes
  - Click "Let's go!"
  - Choose Server Type
    - This Machine 
      - Wait a few seconds
  - Project Setup
    - Click Skip Setup
  - Close pop up forms

### Continue with [Configure Coolify - Setup Wildcard SSL Certificates with Traefik ](coolify/configure-coolify-traefik.md)