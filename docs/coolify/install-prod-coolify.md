# Install & run Coolify on a Virtual Private Server

Following [self hosted installation instructions](https://coolify.io/self-hosted/) for [Coolify](https://coolify.io/)

https://coolify.io/docs/get-started/installation#quick-installation-recommended

ssh to the Virtual Private Server
```bash
ssh root@<my-server.com> -p <ssh-port>
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

You can access Coolify through your Public IPV4: http://<your-public-ip>:8000
You can access Coolify through your Public IPv6: http://<your-public-ipv6>:8000


If your Public IP is not accessible, you can use the following Private IPs:

http://10.0.2.1:8000
http://10.0.0.1:8000
http://10.0.1.1:8000
http://<an-ipv6-here>:8000
http://<another-ipv6-here>:8000

WARNING: It is highly recommended to backup your Environment variables file (/data/coolify/source/.env) to a safe location, outside of this server (e.g. into a Password Manager).
```

- Backup your /data/coolify/source/.env file on the VPS to a safe place
- Open a web browser, and browse to `http://<vps-ip-address>:8000`
  - Fill in the Coolify "Create an account" form, and register your Coolify root user.

- Welcome to Coolify - Connect your first server and start deploying in minutes
  - Click Let's go
  - Choose Server Type
    - Click This Machine
      - Wait ...
      - Port: Enter your VPS ssh port
      - User: root
      - Click Check Again
  - Click "Skip Setup" at the bottom

[2026-04-11 20:07:42] Version: 4.0.0-beta.472
