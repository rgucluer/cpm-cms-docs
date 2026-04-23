# Setup Wildcard SSL Certificates with Traefik

https://coolify.io/docs/knowledge-base/proxy/traefik/wildcard-certs

## Requirements
- You need to have a domain name and a DNS provider that supports wildcard subdomains.
- You need to use [dnsChallenge](https://doc.traefik.io/traefik/https/acme/#dnschallenge) in Traefik to get wildcard certificates from Let's Encrypt.
- You need to use one of the supported DNS providers.
  - Each provider needs environment variables to be set in the Traefik configuration.
  - You can find the required variables in the [official documentation](https://doc.traefik.io/traefik/https/acme/#providers) .

### Ports should be opened for self hosting Coolify
Open the following inbound ports in VPS firewall
```bash
ICMP    ICMP  Any IPv4, Any IPv6
22      TCP   Any IPv4, Any IPv6  (or custom SSH port)
80      TCP   Any IPv4, Any IPv6  ( http )
443     TCP   Any IPv4, Any IPv6  ( https )
443     UDP   Any IPv4, Any IPv6  ( https )
3000    TCP   Any IPv4, Any IPv6  ( Node )
3000    UDP   Any IPv4, Any IPv6  ( Node )
6001    TCP   Any IPv4, Any IPv6  ( Coolify )
6002    TCP   Any IPv4, Any IPv6  ( Coolify )
8000    TCP   Any IPv4, Any IPv6  ( Coolify )
8000    UDP   Any IPv4, Any IPv6  ( Coolify )
8080    TCP   Any IPv4, Any IPv6  ( Traefik )
8080    UDP   Any IPv4, Any IPv6  ( Traefik )
```

---

## Configuration

### Set Coolify Domain

- Coolify Web UI -> Settings -> Configuration -> General
  - Domain: https://coolify.my-domain.com
  - Name: coolify-vps
  - Instance Timezone: UTC
  - Do not change Instance's Public IPv4
  - Save

### Setup General Settings for the Virtual Private Server
- Coolify Web User Interface:
  - Servers -> localhost -> Configuration -> General
    - Name: < servername >
      - server1
    - Wildcard Domain: https://my-domain.com
    - IP Address/Domain: host.docker.internal
    - User: root
    - Port: < vps-ssh-port >
    - Save
    - Validate server if needed.
  - Restart Proxy
  - Wait a few minutes, and close the "Proxy Startup Logs" form.
  - Refresh Page

  - Servers -> < servername > -> Configuration -> Sentinel
    - Coolify URL
      - http://coolify.my-domain.com
      - Check: Enable Sentinel
      - UnCheck: Enable Metrics
    - Save

  - Servers -> < servername > -> Proxy -> Configuration -> Advanced
    - Override default request handler: 
      - Unchecked
    - Save
  - Restart Proxy
  - Wait a few minutes, and close the "Proxy Startup Logs" form.
    - Or close the form if you see the "Successfull ..." message
---

### Set Timezone

List the timezones
```bash
timedatectl list-timezones
```
Navigate with up,down,PgUp,PgDown. Note or copy your selection. Press <kbd>q</kbd> to exit

Set the following values in UI
- Settings -> Configuration -> General -> Instance Timezone
  - Save
- Servers -> < servername > -> Configuration -> General -> Server Timezone  
  - Save

### Set API token for Hetzner if you use Hetzner

- Hetzner Console:
  - Create an API token in the Hetzner Console → choose Project → Security → API Tokens.
    - Generate API Token
      - Description: HETZNER_API_TOKEN
      - Permissions: Read&Write
      - Generate API Token
      - Copy and securely store the value somewhere

- Coolify Web User Interface:
  - Keys & Tokens
    - Cloud Tokens
      - Provider: Hetzner
        - Token Name: HETZNER_API_TOKEN
        - API Token: < Enter your token content here. Get it from Hetzner Console web app >
        - Click - Validate & Add Token
  - Servers -> server1 -> Configuration -> General
    - Restart Proxy

- It may take some time(several minutes) to "Link to Hetzner Cloud" section to appear. Be patient. 

- Coolify Web User Interface:
  - Servers -> server1 -> Configuration -> General
    - Link to Hetzner Cloud (If available)
      - Hetzner Token
        - Choose HETZNER_API_TOKEN
        - Enter your Hetzner Server ID (Get your ID from Hetzner Console)
          - Click Search by ID
          - If a result appears, click "Link This Server"
            - If not, wait a minute try again.



#### Prepare username & password for Traefik Basic Authentication

https://coolify.io/docs/knowledge-base/proxy/traefik/basic-auth#how-to-generate-user-password

On Developer PC
```bash
cd ~
```

```bash
htpasswd users.txt traefikuser
```

Enter a password you choose twice. This will save your traefik user name and encrypted password to user.txt file. Open the file and copy the row you see. We will enter it below as value for traefik.http.middlewares.traefik-basic-auth.basicauth.users .

### Edit Traefik Configuration on VPS
- Coolify Web User Interface:
  - Servers -> < servername > -> Proxy -> Configuration -> Traefik (Coolify Proxy)
  - Add or modify the following sections: 
    - ( ..... represents the ommited sections )
    - < variable-inside > Enter the variable suits to your setup without the angle brackets .
    - Get values from your DNS Provider for
      - For Hetzner
        - DNS: hetzner
        - EMAIL: the e-mail used in DNS provider registration
        - HETZNER_API_TOKEN
      - For detailed information please read
        - Lego Dns Provider list: https://go-acme.github.io/lego/dns/

```yaml
name: coolify-proxy
networks:
  coolify:
    external: true
services:
  traefik:
    container_name: coolify-proxy
    image: 'traefik:v3.6'
    restart: unless-stopped
    environment:
      - 'TZ=Universal'
      - 'EMAIL=< lego-email >'
      - 'DNS=< lego-service-provider-dns >'
      - '< lego-service-provider-env-var >=< lego-service-provider-api-token >'
    extra_hosts:
      - 'host.docker.internal:host-gateway'
    security_opt:
      - 'no-new-privileges=true'
    healthcheck:
      test: 'wget -qO- https://traefik.my-domain.com/ping || exit 1'
      interval: 4s
      timeout: 2s
      retries: 5
      start_period: 6s
    networks:
      - coolify
    ports:
      - '80:80'
      - '443:443'
      - '443:443/udp'
      - '8080:8080'
      - '3000:3000'
    volumes:
      - '/var/run/docker.sock:/var/run/docker.sock:ro'
      - '/data/coolify/proxy/:/traefik'
    command:
      - '--global.checkNewVersion=true'
      - '--ping=true'
      - '--ping.entryPoint=https'
      - '--ping.terminatingStatusCode=204'
      - '--api.dashboard=true'
      - '--api.insecure=false'
      - '--entrypoints.http.address=:80'
      - '--entrypoints.https.address=:443'
      - '--entrypoints.http.http.encodequerysemicolons=true'
      - '--entrypoints.https.http.encodequerysemicolons=true'
      - '--entryPoints.http.http2.maxConcurrentStreams=250'
      - '--entryPoints.https.http2.maxConcurrentStreams=250'
      - '--entrypoints.https.http3'
      - '--providers.docker=true'
      - '--providers.docker.exposedbydefault=false'
      - '--providers.docker.endpoint=unix:///var/run/docker.sock'
      - '--providers.docker.network=coolify'
      - '--providers.file.directory=/traefik/dynamic/'
      - '--providers.file.watch=true'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge=true'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge.provider=hetzner'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge.delaybeforecheck=60'
      - '--certificatesresolvers.letsencrypt.acme.storage=/traefik/acme.json'
      - '--certificatesresolvers.letsencrypt.acme.email=< lego-email >'
      - '--certificatesresolvers.letsencrypt.acme.certificatesDuration=2160'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge.disablePropagationCheck=false'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge.resolvers[0]=213.133.100.102:53'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge.resolvers[1]=213.239.204.242:53'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge.resolvers[2]=193.47.99.4:53'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge.resolvers[3]=1.1.1.1:53'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge.resolvers[4]=8.8.8.8:53'
      - '--accesslog.format=json'
      - '--global.sendAnonymousUsage=false'
    labels:
      - traefik.enable=true
      - coolify.managed=true
      - coolify.proxy=true
```  
Save

Lego Dns Provider list: https://go-acme.github.io/lego/dns/

https://doc.traefik.io/traefik/expose/docker/

### Add Traefik Dynamic Configuration

- Coolify UI -> Servers -> < servername > -> Proxy -> Dynamic Configurations
  - +Add
    - Filename: traefik-dashboard.yml
```yaml
http:
  middlewares:
    authentication:
      basicAuth:
        realm: traefik-dashboard
        users:
          - < traefik-user >:< traefik-enc-password >
    content-type:
      contenttype: true
    gzip:
      compress: true
    redirect-to-https:
      redirectScheme:
        scheme: https
  routers:
    ping-http:
      entryPoints:
        - http
      middlewares:
        - redirect-to-https
      rule: 'Host(`traefik.my-domain.com`)&&PathPrefix(`/ping`)'
      service: noop@internal
    ping-https:
      entryPoints:
        - https
      middlewares:
        - content-type
      rule: 'Host(`traefik.my-domain.com`)&&PathPrefix(`/ping`)'
      service: ping@internal
      tls:
        certResolver: letsencrypt
    traefik-dashboard-http:
      entryPoints:
        - http
      middlewares:
        - redirect-to-https
      rule: 'Host(`traefik.my-domain.com`)&&(PathPrefix(`/api`)||PathPrefix(`/dashboard`))'
      service: noop@internal
    traefik-dashboard-https:
      entryPoints:
        - https
      middlewares:
        - authentication
        - content-type
        - gzip
      rule: 'Host(`traefik.< domain-name >`)&&(PathPrefix(`/api`)||PathPrefix(`/dashboard`))'
      service: api@internal
      tls:
        certResolver: letsencrypt
        domains:
          -
            main: my-domain.com
            sans:
              - '*.my-domain.com'
```
- Save
- Close Form
- Restart Proxy
  - Confirm ... -> Restart Proxy
  - Wait for the process to start ...
    - Close Proxy Startup Logs form
---

### Set DNS Servers

- Settings -> Configuration -> Advanced
  - DNS Settings
    - DNS Validation: Uncheck
  - Custom DNS Servers:
    - Enter IPs of your domain's name servers
  - API Settingss
    - Allowed IPs for API Access
      - < vps-ip >
      - < virtual-m-ip >
    - API Access: Check
  - Save

- Settings -> Configuration -> Advanced
  - DNS Settings
    - DNS Validation: Check
  - Save

- Settings -> Configuration -> Advanced -> UI Settings
  - SPA Navigation: Uncheck
  - Save


---

### Start Proxy or Restart Proxy

- Coolify Web User Interface:
  - Servers -> < servername > -> Start Proxy / Restart Proxy

Modal Form - Proxy Status
```bash
.....
Successfully started coolify-proxy.
Successfully connected coolify-proxy to coolify network.
```
Close Form. If form is unresponsive, wait a few minutes, than close the form.

### Check https://coolify.< domain-name >

- Login

- Change your password
- Close the tab that is using the IP to connect Coolify.

Check https://traefik.< domain-name >/ping

If it returns "OK", Traefik ping is OK .

https://traefik.< domain-name >/dashboard/

Asks for username and password. Enter username (traefikuser) & password you created during "Traefik Basic Authentication" step.

- You can close port 8000 on firewall after successful login via domain name.

---

Later , after adding a App (Resource), read https://coolify.io/docs/knowledge-base/proxy/traefik/wildcard-certs#normal for setting up Traefik settings for your application.

---

## Troubleshooting
- [Get rid of zombies](../troubleshoot/get-rid-of-zombies.md)