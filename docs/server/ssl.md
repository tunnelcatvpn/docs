---
layout: default
title: SSL Tunnel
parent: Server Setup
nav_order: 4
permalink: /server/ssl
last_modified_date: 2020-08-28T14:54:08+0008
---

# Setup SSL Tunnel

TunnelCat VPN uses `stunnel`/`stunnel4` for its SSL tunneling

# Install
Fire up your server terminal. The following commands *requires superuser access*

This is an `Ubuntu` Installation, it might also work in other distro:

```
sudo apt-get update
sudo apt-get install openssl stunnel4 -y
```

Generate self signed certificates first using `openssl` package:
```
openssl genrsa -out key.pem 2048
openssl req -new -x509 -key key.pem -out cert.pem -days 1095
sudo cat key.pem cert.pem >> /etc/stunnel/stunnel.pem
```

Now gain superuser access
```
sudo su
```

Now input this but before you input make sure you made changes first:

```
cat <<EOF > /etc/stunnel/stunnel.conf
client = no
[ovpn]
accept = 0.0.0.0:443
connect = 127.0.0.1:1194
cert = /etc/stunnel/stunnel.pem
EOF
```

1. Replace `0.0.0.0:443` into your desired SSL Tunnel port.

2. Replace `127.0.0.1:1194` into your `OpenVPN` configuration

For Example:
```
cat <<EOF > /etc/stunnel/stunnel.conf
client = no
[ovpn]
accept = 0.0.0.0:8888
connect = 127.0.0.1:443
cert = /etc/stunnel/stunnel.pem
EOF
```

Now enable `stunnel4` to work with systemd and restart the service.

```
sudo sed -i 's/ENABLED=0/ENABLED=1/g' /etc/default/stunnel4
sudo systemctl restart stunnel4
```

To start the service at boot:
```
sudo systemctl enable stunnel4
```