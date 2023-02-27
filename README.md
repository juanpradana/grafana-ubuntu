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

## implement subdomain and SSL for grafana
- `sudo nano /etc/nginx/sites-available/dashboard`
- put this configuration:
```text
server {
        server_name dashboard.yourDomain.com;

        location / {
                proxy_pass http://localhost:4000;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        }

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/yourDomain.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/yourDomain.com/privkey.pem; # managed by Certbot


}

server {
    if ($host = dashboard.yourDomain.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot
    listen 80;
    server_name dashboard.yourDomain.com;
    return 404; # managed by Certbot

}
```
- `sudo ln -s /etc/nginx/sites-available/emqx /etc/nginx/sites-enabled/`
- `sudo certbot --nginx -d yourDomain.com -d www.yourDomain.com -d dashboard.yourDomain.com`
- `sudo systemctl restart nginx`
at your domain panel, add DNS record:
```text
hostname: dashboard
TTL: 14440
Type: A
Value:<your IP Server>
```
