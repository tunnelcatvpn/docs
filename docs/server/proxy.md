---
layout: default
title: HTTP Proxy
parent: Server Setup
nav_order: 2
permalink: /server/proxy
last_modified_date: 2020-08-28T14:54:08+0008
---


# Setup HTTP Proxy

In some cases `TunnelCat VPN` uses `Privoxy` as HTTP Proxy and powered by `OpenHTTP Puncher` in order to work on `HTTP Injection`


Setting up `privoxy` on `Ubuntu 18.04` it might also work on other distro

Install Privoxy, you might be needing `curl` for automated IP address:
```
sudo apt-get update
sudo apt-get install privoxy curl -y
```

Navigate to `privoxy` configuration file *requires superuser access* :
```
cd /etc/privoxy/
sudo nano config
```

Go futher down until you see `listen-address` replace it by
`listen-address :8080`. You can replace `8080` by the port that you wanted.

Now replace the `user.action` in order to use the HTTP Proxy limited to your server service.

Much faster way *requires superuser access* :
```
sudo su
cat <<EOF > /etc/privoxy/user.action
{ +block }
/

{ -block }
`curl ifconfig.co`
127.0.0.1
EOF
```

`curl ifconfig.co` will automatically put your server IP on the configuration. Including `127.0.0.1` is a must when you will use `ohpserver`


Now restart `privoxy` service:

```
systemctl restart privoxy
```

To start the service at boot:
```
systemctl enable privoxy
```


Now, proceed to OpenHTTP Puncher installation