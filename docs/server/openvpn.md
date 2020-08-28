---
layout: default
title: OpenVPN
parent: Server Setup
nav_order: 1
permalink: /server/openvpn
last_modified_date: 2020-08-28T14:54:08+0008
---


# Setup OpenVPN

 There's several setup script out there to make you OpenVPN setup easy. I recommend to use:


[https://github.com/Nyr/openvpn-install](https://github.com/Nyr/openvpn-install)

Fire up your `Console` and run the command and follow the instructions:
```
wget https://git.io/vpn -O openvpn-install.sh && bash openvpn-install.sh
```

Just make sure you setup your `OpenVPN` on `TCP` mode not in `UDP`

## Making your OpenVPN server accessible by one certificate
Navigate to your OpenVPN Server configuration folder.
This is only tested on `Ubuntu 18.04` it might also work on other distro

You might be needing `superuser` rights inorder to modify the `server.conf`. Use `sudo`

```
cd /etc/openvpn/server
sudo nano server.conf
```

Add this on the end of the file
```
duplicate-cn
```

Example:
```
port 1194
proto tcp
dev tun
ca ca.crt
cert server.crt
key server.key
dh dh.pem
topology subnet
server 10.8.0.0 255.255.255.0
push "redirect-gateway def1 bypass-dhcp"
ifconfig-pool-persist ipp.txt
push "dhcp-option DNS 1.1.1.1"
push "dhcp-option DNS 1.0.0.1"
keepalive 10 120
cipher AES-128-CBC
user nobody
group nogroup
persist-key
persist-tun
status openvpn-status.log
verb 3
duplicate-cn
```

Finally save the file by hitting `CTRL + O` then `ENTER`  on your keyboard