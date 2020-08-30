---
layout: default
title: OpenHTTP Puncher
parent: Server Setup
nav_order: 3
permalink: /server/ohp
last_modified_date: 2020-08-29T17:03:08+0008
---


# Setup OpenHTTP Puncher

`OpenHTTP Puncher` a tool that helps you to bypass internet firewall using HTTP.
This will force your server to send HTTP connection established response.


# Features
1. Upgrades your HTTP Proxy software `squid`/`tinyproxy`/`privoxy` to an HTTP tunneling friendly 
2. Allows invalid HTTP request to be sent to the server and respond `200 HTTP Status Code`

# Install

Go to your `SSH` client or `Console` and establish a connection with your server on the terminal input the following command:

Some linux distrobution doesn't have `unzip` installed. So you must install it first by using:

`sudo apt install unzip -y` on `Ubuntu`

Now input this command:
```
wget https://github.com/lfasmpao/open-http-puncher/releases/download/0.1/ohpserver-linux32.zip
unzip ohpserver-linux32.zip
chmod 755 ohpserver
sudo mv ohpserver /usr/local/bin/
```

Now the `ohpserver` is installed on your distro.

You can verify it by typing `ohpserver` and it will output:
```
Open HTTP Puncher Server by @lfasmpao
Version: 0.1 beta
Released on: 4/24/2020
  -port int
        Server Port (default 5555)
  -proxy string
        Proxy Server [host:port]
  -tunnel string
        Tunnel Server [host:port]
```
# Running the Server

In your terminal:
`ohpserver -port 81 -proxy 127.0.0.1:8080 -tunnel 127.0.0.1:1194`

You can change `-port 81` into your desired port.

Change `-proxy 127.0.0.1:8080` with your `privoxy` configuration. For instance your setup `privoxy` on port `3128` change it into `-proxy 127.0.0.1:3128`

Change `-tunnel 192.168.8.1:1194` with your `OpenVPN` configuration. For instance you setup `OpenVPN` on TCP port `443` change it into `-tunnel 192.168.8.1:1194`

You must change `192.168.8.1` with your `Server IP`

For example:
I have a server that has an IP of `1.1.1.1` and then a `privoxy` setup on `3128` and an OpenVPN server running on `TCP` port `5555` then here is the command that you'll be using:

`ohpserver -port 81 -proxy 127.0.0.1:3128 -tunnel 1.1.1.1:5555`

Congratulations the server is running!

Now how about running it on start up and daemonize it

# Daemonize

*This requires superuser access*

Fire up your nano and create a `ohpserver.service` in `/etc/systemd/system/`


```
sudo nano /etc/systemd/system/ohpserver.service
```

Input:
```
[Unit]
Description=Daemonize OpenHTTP Puncher Server
Wants=network.target
After=network.target

[Service]
ExecStart=/usr/local/bin/ohpserver -port 81 -proxy 127.0.0.1:3128 -tunnel 1.1.1.1:5555
Restart=always
RestartSec=3
[Install]
WantedBy=multi-user.target
```

![](/assets/images/{0D620CE4-66C9-40BB-9F95-CF372063AA6C}.png)

Now press `CTRL + O` and `ENTER` on your keyboard

Change `-port 81 -proxy 127.0.0.1:3128 -tunnel 1.1.1.1:5555` into your setup.

Now reload the `systemctl` and start it
```
sudo systemctl daemon-reload
sudo systemctl start ohpserver
```

Verify if the start is successfull
```
sudo systemctl status ohpserver
```

![](/assets/images/{7D2A501F-3651-4B26-AB9F-0A0F37EBDE7D}.png)

To start the service at boot:
```
sudo systemctl enable ohpserver
```