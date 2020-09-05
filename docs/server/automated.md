---
layout: default
title: Automated Setup
nav_order: 7
has_children: true
permalink: /automated-setup
last_modified_date: 2020-09-05T20:38:00+0008
---


# Automated Setup

# Prerequisite
1. Linux VPS
2. Ubuntu/Debian OS
3. Domain Setup for Protocol G
4. OpenVPN Installed (TCP)

# Setup Domain for Protocol G
If you are going to put Protocol G you must setup your domain first to use dns2tcp.

You can follow this [tutorial](https://docs.tcat.me/server/slowdns#setup-ns-records-with-cloudflare) for the setup

# Install OpenVPN
I recommend to use [https://github.com/Nyr/openvpn-install](https://github.com/Nyr/openvpn-install)

Install OpenVPN using this script:
```
wget https://git.io/vpn -O openvpn-install.sh && bash openvpn-install.sh
```

*You must install OpenVPN on TCP mode*

# Running the Script
After installing OpenVPN run this command and follow the installation:
```
wget https://github.com/tunnelcatvpn/setup/raw/master/setup.sh -O setup.sh && sudo bash setup.sh
```

# Uninstall
Run this command:
```
wget https://raw.githubusercontent.com/tunnelcatvpn/setup/master/uninstall.sh -O uninstall.sh && sudo bash uninstall.sh
```

The GitHub repository is available [here](https://github.com/tunnelcatvpn/setup)
