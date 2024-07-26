## Uninstall Loki on Ubuntu.
To completely remove the Loki, stop the service and remove a systemd unit file.
```
sudo service loki stop
sudo systemctl disable loki
sudo rm -rf /etc/systemd/system/loki.service
sudo systemctl daemon-reload
sudo systemctl reset-failed
```

### Delete the installation directory:
```
sudo rm -rf /opt/loki
```

### Remove symbolic link:
```
sudo rm -rf /usr/local/bin/loki
```
