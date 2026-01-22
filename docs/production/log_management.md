# Log Management

**Check total log directory size**

```bash
sudo du -sh /var/log
```

**List the log files**

```bash
sudo du -sh /var/log/* | sort -hr | head -10
```

## Configure Log Rotation

Main logrotate configuration
```bash
sudo cat /etc/logrotate.conf
```

List application-specific rotation rules
```bash
sudo ls -la /etc/logrotate.d/
```

Edit global settings
```bash
sudo nano /etc/logrotate.conf
```

```bash
# see "man logrotate" for details
daily  
#weekly 
#monthly
su root adm
rotate 7
create
include /etc/logrotate.d
```

## Clear system logs
Clear all journal logs older than a specific time period.
```bash
sudo journalctl --vacuum-time=7d
```

## Configure persistent journal size limits

Use 'systemd-analyze cat-config systemd/journald.conf' to display the full config.

mkdir /etc/systemd/journald.conf.d/
cp /etc/systemd/journald.conf /etc/systemd/journald.conf.d/

```bash
nano /etc/systemd/journald.conf.d/journald.conf
```

```ini
[Journal]
SystemMaxUse=2G
SystemMaxFileSize=1G
```

Save & Exit

sudo systemctl restart systemd-journald


## References
- https://www.interserver.net/tips/kb/ubuntu-log-file-management-clear-system-logs-safely/
