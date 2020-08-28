---
layout: default
title: Slow DNS
parent: Server Setup
nav_order: 5
permalink: /server/slowdns
last_modified_date: 2020-08-28T14:54:08+0008
---

# Setup Slow DNS
TunnelCat VPN now uses an open-source DNS tunneling `dns2tcp` for its Slow DNS implementation

# Prerequisites

You must setup a domain with NS records linked to your server.

# Setup NS records with Cloudflare:
TunnelCat VPN uses `Cloudflare` services on its domain

On your domain portal, navigate to DNS:

![](/assets/images/{261642E4-40E2-4E03-B52A-05C06384A893}.png)

Add Record:

![](/assets/images/{7C55C055-2170-4262-9393-5C12999B2FEB}.png)

Put your server IP address on `A Record`:

![](/assets/images/{10B9C8F3-1DE9-4AC2-BAFA-ECC160DD5E6F}.png)

Now create another `Record` on `NS`:

![](/assets/images/{2E78ABB7-C159-4786-BDED-FA1810F44AD8}.png)

Put your `A Record` that you created earlier, put its domain name on the nameserver. Remember the `NS Record Name` as you need it for the tunnel

**Please note that you must create `NS Record Name` with `dns.` as prefix.**

For instance: `tunnel` should be `dns.tunnel` as the application adds `dns.` on the domain on the repository

![](/assets/images/{125D6921-0C2C-4A35-96D1-C87E6457051D}.png)

# Install

On `Ubuntu`:
```
sudo apt-get update
sudo apt-get install dns2tcp -y
```

Now create a configuration folder at `/etc/` and configuration file:

```
sudo mkdir /etc/dns2tcp/
sudo nano /etc/dns2tcp/server.conf
```


Now put the configuration:
```
listen = YOUR_SERVERIP
port = 53
user = nobody
chroot = /var/empty/dns2tcp/
pid_file = /var/run/dns2tcp.pid
domain = YOUR_NS_RECORD_NAME
resources = ovpn:YOUR_OPENVPN_IP_PORT
```

Example:
```
listen = 127.0.0.1
port = 53
user = nobody
chroot = /var/empty/dns2tcp/
pid_file = /var/run/dns2tcp.pid
domain = dns.tunnel.tcat.me
resources = ovpn:127.0.0.1:1194
```

And then save the configruation by hitting `CRTL + O` then `ENTER` on your keyboard

# Run the Tunnel

You can run the tunnel daemon by:
```
sudo dns2tcpd -d 1 -f /etc/dns2tcp/server.conf
```

In some cases you might be seeing `bind` error
You must stop the `systemd-resolved`
```
systemctl stop systemd-resolved
```

Now enable Slow DNS on your Server on Server Repository
