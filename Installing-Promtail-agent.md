# Installing Promtail agent.
Pomtail is an agent which ships the contents of local logs to a private Grafana Loki instance or Grafana cloud and it is usually deployed to every machine that has applications needed to be monitored.

### Go to its release page on GitHub and download Promtail binary zip file –
```
curl -LO https://github.com/grafana/loki/releases/download/v2.4.2/promtail-linux-amd64.zip
```

### Once the package is downloaded extract the package to /usr/local/bin –

```
unzip promtail-linux-amd64.zip
sudo mv promtail-linux-amd64 /usr/local/bin/promtail
```
### Now create a YAML configuration file for Promtail in the /usr/local/bin directory –
```
sudo nano /etc/promtail-local-config.yaml
```
### Add the given lines to this file –
```
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /data/loki/positions.yaml

clients:
  - url: http://localhost:3100/loki/api/v1/push

scrape_configs:
- job_name: system
  static_configs:
  - targets:
      - localhost
    labels:
      job: varlogs
      __path__: /var/log/*log

```

### Next create a service file for Promtail –
```
sudo tee /etc/systemd/system/promtail.service<<EOF
[Unit]
Description=Promtail service
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/promtail -config.file /etc/promtail-local-config.yaml

[Install]
WantedBy=multi-user.target
EOF
```
### Reload system daemon –

```
sudo systemctl daemon-reload
```
### And start Promtail services –

```
sudo systemctl start promtail.service
```
### You can also check the promtail service status by using –
```
systemctl status promtail.service

![image](https://github.com/user-attachments/assets/d4e11ef1-eed7-41c6-90c0-0fc0349aa281)
```




