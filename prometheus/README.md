# prometheus
- premetheus download urls
```https://prometheus.io/download/```
` https://github.com/prometheus/prometheus/releases/download/v2.13.1/prometheus-2.13.1.linux-amd64.tar.gz `

# prometheus Installation
- Preparing your Environment
To run Prometheus safely on our server, we have to create a user for Prometheus and Node Exporter without the possibility to log in. To achieve this, we use the parameter `--no-create-home` which skips the creation of a home directory and disable the shell with `--shell /usr/sbin/nologin`
```
$ sudo useradd --no-create-home --shell /usr/sbin/nologin prometheus
```
Create the folders required to store the binaries of Prometheus and its configuration files:
```
$ sudo mkdir /etc/prometheus
$ sudo mkdir /var/lib/prometheus
```
Set the ownership of these directories to our `prometheus` user, to make sure that Prometheus can access to these folders:
```
sudo chown prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus
```
