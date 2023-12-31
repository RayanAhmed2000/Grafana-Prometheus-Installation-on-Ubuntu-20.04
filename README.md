# Grafana - 2 ways 
1 - Either by using Docker i.e. running grafana container
2 - By using Pre build binaries and installing grafana on Local machine

# Using Docker
- Install docker on your machine
- pull grafana image and run conatainer
```
docker run -d -p 3000:3000 --name=grafana -v grafana-storage:/var/lib/grafana grafana/grafana
```
- above command will create a conatiner, map its port 3000 to 3000 of local machien and create a volume named grafana-storage so that dashboards are preserved when container restarts


# Grafana-Installation-on-Ubuntu-20.04
- Run the following commands
```
sudo apt-get install -y adduser libfontconfig1 musl
wget https://dl.grafana.com/enterprise/release/grafana-enterprise_10.1.5_amd64.deb
sudo dpkg -i grafana-enterprise_10.1.5_amd64.deb

```
- start the service
```
service grafana-server start
```
- if you face error try fix the broken package
```
apt --fix-broken install -y
```
- now again start the service
```
service grafana-server start
```
# Prometheus-Installation-on-Ubuntu-20.04
- Creating user, group and directories
```
groupadd prometheus
useradd -s /sbin/nologin --system -g prometheus prometheus
mkdir /var/lib/prometheus
for i in rules rules.d files_sd; do sudo mkdir -p /etc/prometheus/${i}; done

```
- Now we install Prometheus on Ubuntu
```
apt install curl
mkdir -p /tmp/prometheus
cd /tmp/prometheus
curl -s https://api.github.com/repos/prometheus/prometheus/releases/latest | grep browser_download_url | grep linux-amd64 | cut -d '"' -f 4 | wget -qi -
tar xvf prometheus*.tar.gz
cd /tmp/prometheus/prometheus-2.46.0.linux-amd64
mv prometheus promtool /usr/local/bin/
mv prometheus.yml /etc/prometheus/prometheus.yml
mv consoles/ console_libraries/ /etc/prometheus/
nano /etc/prometheus/prometheus.yml

```
- Creating the Prometheus Systemd service
```
nano /etc/systemd/system/prometheus.service
```
- Add this text to this file:
```
[Unit]
Description=Prometheus
Documentation=https://prometheus.io/docs/introduction/overview/
Wants=network-online.target
After=network-online.target
[Service]
Type=simple
User=prometheus
Group=prometheus
ExecReload=/bin/kill -HUP \$MAINPID
ExecStart=/usr/local/bin/prometheus \
--config.file=/etc/prometheus/prometheus.yml \
--storage.tsdb.path=/var/lib/prometheus \
--web.console.templates=/etc/prometheus/consoles \
--web.console.libraries=/etc/prometheus/console_libraries \
--web.listen-address=0.0.0.0:9090 \
--web.external-url=
SyslogIdentifier=prometheus
Restart=always
[Install]
WantedBy=multi-user.target
```
- And finally, change the owner of these directories to the previously created user and Prometheus group:

```
for i in rules rules.d files_sd; do sudo chown -R prometheus:prometheus /etc/prometheus/${i}; done
for i in rules rules.d files_sd; do sudo chmod -R 775 /etc/prometheus/${i}; done
chown -R prometheus:prometheus /var/lib/prometheus/
```
- Once you have spelled and checked everything out, reboot systemd:
```
systemctl daemon-reload
systemctl enable prometheus
```
- Configure the firewall
```
ufw allow in "Nginx Full"
ufw allow 9090/tcp
```
- To know more [click here](https://serverspace.io/support/help/install-prometheus-ubuntu-20-04/)




