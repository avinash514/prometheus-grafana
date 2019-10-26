# prometheus
- premetheus download urls
```https://prometheus.io/download/```
` https://github.com/prometheus/prometheus/releases/download/v2.13.1/prometheus-2.13.1.linux-amd64.tar.gz `

# prometheus Installation
### Preparing your Environment
- To run Prometheus safely on our server, we have to create a user for Prometheus and Node Exporter without the possibility to log in. To achieve this, we use the parameter `--no-create-home` which skips the creation of a home directory and disable the shell with `--shell /usr/sbin/nologin`
```
$ sudo useradd --no-create-home --shell /usr/sbin/nologin prometheus
```
- Create the folders required to store the binaries of Prometheus and its configuration files:
```
$ sudo mkdir /etc/prometheus
$ sudo mkdir /var/lib/prometheus
```
- Set the ownership of these directories to our `prometheus` user, to make sure that Prometheus can access to these folders:
```
sudo chown prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus
```
### Installation
- Downloading and Extracting
```
wget https://github.com/prometheus/prometheus/releases/download/v2.2.1/prometheus-2.2.1.linux-amd64.tar.gz
tar xfz prometheus-*.tar.gz
cd prometheus-*
```
- Copy the binary files into the `/usr/local/bin/` directory:
```
sudo cp ./prometheus /usr/local/bin/
sudo cp ./promtool /usr/local/bin/
```
- Set the ownership of these files to the `prometheus` user previously created:
```
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool
```
- Copy the `consoles` and `console_libraries` directories to `/etc/prometheus`:
```
sudo cp -r ./consoles /etc/prometheus
sudo cp -r ./console_libraries /etc/prometheus
```
-Set the ownership of the two folders, as well as of all files that they contain, to our `prometheus` user:
```
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
```
# Configuring Prometheus
- Open the file `prometheus.yml` in a text editor:
``` sudo nano /etc/prometheus/prometheus.yml ```
- Paste below content
```
global:
  scrape_interval:     15s
  evaluation_interval: 15s

rule_files:
  # - "first.rules"
  # - "second.rules"

scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9090']
```
- Set the ownership of the file to our `Prometheus` user:
```
sudo chown prometheus:prometheus /etc/prometheus/prometheus.yml
```
