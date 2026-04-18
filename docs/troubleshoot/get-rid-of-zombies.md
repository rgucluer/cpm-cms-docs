
### Lots of zombie 'ssh-clients' processes

TODO: This guide is incomplete.

After ssh connection, we get notification about zombie processes in System information

```bash
......
=> There are 243 zombie processes.
```

top application also show zombie process count.

- List zombie processes on VPS
```bash
ps aux | grep 'Z'
```

Find parent of a process
```bash
pstree -p -s <process_id>
```

```bash
systemd(1)───containerd-shim(33244)───traefik(33269)───ssl_client(190319)
```

```bash
sudo journalctl -u ssh
```

-Lots of connections from 10.0.1.6. This IP in my situation points to coolify container.
  - https://github.com/coollabsio/coolify/issues/4769


---

- Adjust SSH KeepAlives
  - Edit /etc/ssh/sshd_config on VPS
```bash
.....
ClientAliveInterval 60
ClientAliveCountMax 3
.....
```
  
```bash
sudo systemctl restart ssh
```

https://github.com/coollabsio/coolify/issues/4769

### Enable Sentinel:
- Coolify UI -> Servers -> <servername> 
  - Enable Sentinel
  - Do NOT check : Enable Metrics
  - Save
  - Restart Proxy
- Check zombies
- Zombies still increasing but I will not revert. Let this stay enabled.

## Did not work

### Add init option to coolify service 
- ssh to VPS
```bash
cd /data/coolify/source
```

- docker-compose.yml

```yaml
services:
  coolify:
    init: true
    container_name: coolify
    .....
```
Save&Exit

Revert back

---

### Change ssh options in VPS
ssh to server (VPS)

nano /root/.ssh/config

```bash
Host *
    # Send a keepalive every 10 seconds
    ServerAliveInterval 10
    # Disconnect after 3 missed keepalives (30 seconds total)
    ServerAliveCountMax 3
    # Automatically close multiplexed masters after 1 minute of idleness
    ControlMaster auto
    ControlPersist 1m
```

- Save & Exit
- Reboot
- ssh to server
  - Still got zombies
  - wait a bit 
  - Still same

Revert back

---