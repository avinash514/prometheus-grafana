# node-exporter
- node-exporter download url
` https://github.com/prometheus/node_exporter/releases/download/v0.18.1/node_exporter-0.18.1.linux-amd64.tar.gz `

## Preparing your Environment
- Create node-exporter user with `--no-create-home` which skips the creation of a home directory
```
sudo useradd --no-create-home --shell /bin/false node_exporter
```
## Downloading and Installing Node Exporter
As your Prometheus is only capable of collecting metrics, we want to extend its capabilities by adding Node Exporter, a tool that collects information about the system including CPU, disk, and memory usage and exposes them for scraping.

- Download the latest version of Node Exporter:
```
wget https://github.com/prometheus/node_exporter/releases/download/v0.18.1/node_exporter-0.18.1.linux-amd64.tar.gz
```
- Unpack the downloaded archive. This will create a directory `node_exporter-0.18.1.linux-amd64`, containing the executable, a readme and license file:
```
tar xvf node_exporter-0.18.1.linux-amd64.tar.gz
```
- Copy the binary file into the directory `/usr/local/bin` and set the ownership to the user you have created in step previously:
```
sudo cp node_exporter-0.18.1.linux-amd64/node_exporter /usr/local/bin
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```
- Remove the leftover files of Node Exporter, as they are not needed any longer:
```
rm -rf node_exporter-0.18.1.linux-amd64.tar.gz node_exporter-0.18.1.linux-amd64
```
- To run Node Exporter automatically on each boot, a Systemd service file is required. Create the following file by opening it in Nano:
```
sudo vi /etc/systemd/system/node_exporter.service
```
- Copy the following information in the service file, save it and exit Nano:
```
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```
- Reload Systemd to use the newly defined service:
```
sudo systemctl daemon-reload
```
- Run Node Exporter by typing the following command:
```
sudo systemctl start node_exporter
```
- Verify that the software has been started successfully:
```
sudo systemctl status node_exporter
```
- You will see an output like this, showing you the status ``active (running)`` as well as the main PID of the application:
```
● node_exporter.service - Node Exporter
   Loaded: loaded (/etc/systemd/system/node_exporter.service; disabled; vendor preset: enabled)
   Active: active (running) since Mon 2018-06-25 11:47:06 UTC; 4s ago
 Main PID: 1719 (node_exporter)
   CGroup: /system.slice/node_exporter.service
           └─1719 /usr/local/bin/node_exporter
```
- If everything is working, enable Node Exporter to be started on each boot of the server:
```
sudo systemctl enable node_exporter
```
- Add below config to prometheus.yml file
```
- job_name: 'node_exporter'
  scrape_interval: 5s
  static_configs:
    - targets: ['localhost:9100']
```
