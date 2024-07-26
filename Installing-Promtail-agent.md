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
### Configuring Loki data source
You have setup everything now it time to link loki to Grafana monitoring tool. Use the given URL to open Grafana login page –
```
http://localhost:3000
```

### Use username and password as admin.
Once logged in successfully you will be able to see the Grafana dashboard.

![image](https://github.com/user-attachments/assets/d6c7f144-ca2f-4b51-8b6f-25de81509c48)
Go to Configuration> Data sources> Add Data Source now find and click on Loki.

Add the following settings to this page –

Name – Loki

URL – http://localhost:3100

As you can see in the image below.
![image](https://github.com/user-attachments/assets/dbdfc3a7-0a0a-476f-bebf-80d2c38aae60)

Scroll down and click on Save & test.

Visualizing logs in Grafana Loki
To visualize logs in Grafana Loki go to Exaplore and then select Loki at the data source. In the log browser enter a Loki query for example to view varlogs enter {job=”varlogs”}.

Now you can scroll and view logs displayed in the dashboard.

![image](https://github.com/user-attachments/assets/1d053b50-4bcb-4f09-832a-4f69ed3da050)

### Conclusion
I hope you have setup Grafana Loki successfully on your Ubuntu system. Now if you have a query then write us in the comments below.







