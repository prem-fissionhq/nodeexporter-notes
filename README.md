```
sudo groupadd -f node_exporter;
sudo useradd -g node_exporter --no-create-home --shell /bin/false node_exporter;
sudo mkdir /etc/node_exporter;
sudo chown node_exporter:node_exporter /etc/node_exporter;
```

# Download the Node exporter 
[Download](https://prometheus.io/download/) the Node Exporter binary to each Couchbase Server that you want to monitor. The Node Exporter will export system related stats. 

```
wget https://github.com/prometheus/node_exporter/releases/download/v1.8.0/node_exporter-1.8.0.linux-amd64.tar.gz

tar -xvf node_exporter-1.8.0.linux-amd64.tar.gz
mv node_exporter-1.8.0.linux-amd64 node_exporter-files

sudo cp node_exporter-files/node_exporter /usr/bin/
sudo chown node_exporter:node_exporter /usr/bin/node_exporter
```

# Create the service file
sudo vi /usr/lib/systemd/system/node_exporter.service


```
[Unit]
Description=Node Exporter
Documentation=https://prometheus.io/docs/guides/node-exporter/
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
ExecStart=/usr/bin/node_exporter \
  --web.listen-address=:9100

[Install]
WantedBy=multi-user.target
```

```
sudo chmod 664 /usr/lib/systemd/system/node_exporter.service
```

# reload systemctl 
sudo systemctl daemon-reload
sudo systemctl start node_exporter

sudo systemctl enable node_exporter.service
sudo systemctl status node_exporter
