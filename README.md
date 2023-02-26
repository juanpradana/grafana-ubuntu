# grafana-ubuntu
This running on ubuntu server 22.04

## installation
- `sudo apt-get update`
- `sudo apt-get install -y apt-transport-https`
- `sudo apt-get install -y software-properties-common wget`
- `sudo wget -q -O /usr/share/keyrings/grafana.key https://apt.grafana.com/gpg.key`
- `echo "deb [signed-by=/usr/share/keyrings/grafana.key] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list`
- `sudo apt-get update`
- `sudo apt-get install grafana`

### change port configuration
- `sudo nano /etc/grafana/grafana.ini`
- change:
```text
# The http port  to use
;http_port = 3000
```
to
```text
# The http port  to use
http_port = 4000
```
- change"
```text
[auth.anonymous]
# enable anonymous access
;enabled = false
```
to
```text
[auth.anonymous]
# enable anonymous access
enabled = true
```

### start and check grafana status
- `sudo systemctl start grafana-server`
- `sudo systemctl status grafana-server`
- `sudo systemctl enable grafana-server.service` to enable starup

## access grafana
open host:4000 on browser. default login is `admin` `admin`

## build dashboard
- install plugin: Infinity (Visualize data from JSON, CSV, XML, GraphQL and HTML endpoints)
