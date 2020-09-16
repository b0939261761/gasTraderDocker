# Gas Trader

## Docker certbot

```bash
sudo docker run \
  --rm \
  --name certbot \
  --volume "$(pwd)/certbot/conf:/etc/letsencrypt" \
  --volume "$(pwd)/certbot/www:/var/www/certbot" \
  --volume "$(pwd)/cloudflare.ini:/root/.secrets/certbot/cloudflare.ini" \
  certbot/dns-cloudflare certonly \
  --manual-public-ip-logging-ok \
  --agree-tos \
  --domains gas.reality.sh \
  --email b360124@gmail.com \
  --server https://acme-v02.api.letsencrypt.org/directory \
  --dns-cloudflare \
  --dns-cloudflare-credentials /root/.secrets/certbot/cloudflare.ini
```

## Create service file

```bash
vi /lib/systemd/system/gasTrader.service
```

```ini
[Unit]
Description=Gas Trader Portal
After=docker.service
Conflicts=shutdown.target reboot.target halt.target

[Service]
Restart=always
RestartSec=10
WorkingDirectory=/root/gasTrader/gasTraderDocker
ExecStart=/usr/local/bin/docker-compose up
ExecStop=/usr/local/bin/docker-compose down
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
TimeoutStartSec=10
TimeoutStopSec=30
StartLimitBurst=3
StartLimitInterval=60s
NotifyAccess=all

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl enable gasTrader
sudo systemctl start gasTrader
```


