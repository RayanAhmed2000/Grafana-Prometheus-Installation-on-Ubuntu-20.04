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
