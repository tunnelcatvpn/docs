---
layout: default
title: Repository
nav_order: 2
permalink: /repository
last_modified_date: 2020-08-28T14:54:08+0008
---


# Setting up your own repository
You need to create an account at:

[Control Panel](https://cp.tcat.me){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

The Repository System is only available on `TunnelCat VPN` version `2.3.0` above

## Adding Server Into Your Repository
You might be needing to setup your own server. The tutorial is available [here](/server-setup)

Once your server is setup you can try to `Add Server`

On `Add Server` editor you might be seeing lots of inputs. More depth information about the inputs:

1. Name: The name that will appear on the application server menu
2. IP Address: The IP of the server
3. Domain: You need to setup a domain in order to use `Slow DNS or Protocol G`
4. OpenVPN Port: Your OpenVPN server port must be `TCP`, you only need `UDP` if you have `Fast DNS or Protocol F` Installed
5. HTTP Proxy Port: Your `squid`/`privoxy` exposed port
6. OpenHTTP Puncher Port: Your `ohpserver` installation port
7. SSL Port: Your `stunnel` installation port
8. Fast DNS Installed: Check if `fastdns_server` is instaled
9. Slow DNS Installed: Check if `dns2tcp` is installed
10. HTTP Tunnel Installed: Check if `htunserver` is installed

### OpenVPN Options
Here you should put you OpenVPN configuration options


Example:
```
client
dev tun
proto tcp
resolv-retry infinite
nobind
persist-key
persist-tun
remote-cert-tls server
cipher AES-128-CBC
verb 3
```

Notice that there is no `remote` on the configuration as the application setup this automatically. You **don't need to include** this.

Valid configurations:

```
client
dev tun
proto tcp
resolv-retry infinite
nobind
persist-key
persist-tun
remote-cert-tls server
<auth-user-pass>
user1
user1
</auth-user-pass>
auth none
cipher none
verb 3
```

```
client
dev tun
proto tcp
resolv-retry infinite
nobind
persist-key
persist-tun
remote-cert-tls server
cipher AES-256-CBC
verb 3
```

Invalid configuration:
```
client
dev tun
proto tcp
resolv-retry infinite
nobind
persist-key
persist-tun
remote-cert-tls server
auth-user-pass
auth none
cipher none
verb 3
```
You must not include `auth-user-pass` as it requires user input on the application. Instead use:
```
<auth-user-pass>
user1 # Username
user1 # Password
</auth-user-pass>
```

=========================== 

Also the Repository provides steamless updates to the user with authentication, you may add to the repository on the application:

`@lfasmpao?account=username:password`

Please note that `username:password` must be base64 encoded:

Example:
`@lfasmpao?account=dXNlcm5hbWU6cGFzc3dvcmQ`

Note: This feature requires an upgraded account

=========================== 

Some options you **not** must be using:

`dns` `route`

### OpenVPN Key
This the most crucial part of the server make sure you put the correct information

You should put here the `<ca></ca>` `<key></key>` `<cert></cert>` `<tls-auth></tls-auth>`

Example input:
```
<ca>
-----BEGIN CERTIFICATE-----
MIIDQjCCAiqgAwIBAgIUPnUY05tNGCw9Veb0ItE6Iwjc6AYwDQYJKoZIhvcNAQEL
BQAwEzERMA8GA1UEAwwIQ2hhbmdlTWUwHhcNMjAwNzMwMTQ1NjUyWhcNMzAwNzI4
MTQ1NjUyWjATMREwDwYDVQQDDAhDaGFuZ2VNZTCCASIwDQYJKoZIhvcNAQEBBQAD
ggEPADCCAQoCggEBANPKyp2BTr5JkFuk7/Efc3rdyA7Ol8vEU+0wYO/8+EKhKvuP
16iBwpcByxhrdB1eddxs1/P1VoZIbWa+GOFTeaO1Lf073soUsSmmnpLHceJTFhzA
CbT6RenE9Y0CUM/W0Rbm+/9iqyg5Rqc7siYnjDXEHpdj6Stwzr3g0oQlJ+6jp2vv
XIQTyiybU5aAvYBDRSsGxVO4UHDm4uzRAgOzC+z5tn/8XV1UaVEyf+wD50N3WO+B
St4k1m3idYLBGnum3OceGouPQdFLXMZLPv1xtp+YXMjFBXCgEO+AbeBz/wQGpMQQ
yyrqcSntwjslWue+XtuIcKkoso55fm1OHKrJO30CAwEAAaOBjTCBijAdBgNVHQ4E
FgQUzEUZDU9V/bhyJoepV4IMCCK59O8wTgYDVR0jBEcwRYAUzEUZDU9V/bhyJoep
V4IMCCK59O+hF6QVMBMxETAPBgNVBAMMCENoYW5nZU1lghQ+dRjTm00YLD1V5vQi
0TojCNzoBjAMBgNVHRMEBTADAQH/MAsGA1UdDwQEAwIBBjANBgkqhkiG9w0BAQsF
AAOCAQEAP89wa49PjjGEldmhi8viunV/nOBjJ1bsGLNq9Zmq8xMrLyHJeXdZiwCi
48sSEdLZAvIZkQ/J9cbEMDWmRLmD38gOrognHthfaHcSIpMQzBbw115GUkshaXVQ
efOW40/Sn59cYZhCOGkYDJvW+siy6gdgjuwX5HL1SSiTxFxlTzDbUFirCcDtgTh8
anzoKFLuxPpG2q6KBuHtPa8EBXpTVd3N5heV6Er75HaDRvSvOUVSkbOafKjkF6jx
QSEPYoxRM2aZO7pkyZJX1FymKfWmKvbjiOyD4F/tjCBouzar+ozae2/cZa0M2zi3
azw4adkkDMkrDMkIxsC8tJ0cinfwdw==
-----END CERTIFICATE-----
</ca>
<key>
-----BEGIN PRIVATE KEY-----
(truncated)
-----END PRIVATE KEY-----
</key>
<cert>
Certificate:
(truncated)
-----BEGIN CERTIFICATE-----
(truncated)
-----END CERTIFICATE-----
</cert>

```

**Don't include any OpenVPN configuration options here**

After setting up your server you need to `Publish` the list to your user and it will be automatically available on users. They must set the application `Repository Username` under the application `Settings` to your account's username