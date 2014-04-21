---
layout: post
title:  "Accessing internal servers remotely"
date:   2012-09-04 13:17:50
author: Michael
---
Over at [Ground Six](http://www.groundsix.com) the development team each run a number of virtual machines hosting their development environment. We also have an internal ESXi server which hosts a number of projects and staging areas.

For our local machines, tools such as [LocalTunnel](http://progrium.com/localtunnel) and [PageKite](https://pagekite.net/) are great. These are free services which tunnel out and expose a particular port (for us this is always the web server) on a subdomain they provide. This is essential for testing services with webhooks such as PayPal or GoCardless.

Both of these services are very good and customisable. The v2 of LocalTunnel and PageKite offer some facility to run your own service or front-end without needing to rely on their own servers. Unfortunately, LocalTunnel v2 is very much unstable software, and PageKites documentation is a little overwhelming (although I'm told their rpm and deb files help with that). With this in mind I set about putting something (very basic) together myself, primarily for the servers we have running on our ESXi box; PageKite is more than suitable for our local machines for now.

## How it works

One thing I didn't realise with SSH, was that when you connect from one machine to another, you can specify a port binding, that allows you to bind a port on one machine to another. This binding can work in one of two ways:

- Local: You can connect to a remote machine and bind a port from that machine to a local port
- Remote: You can connect to a remote machine and bind a port from the local machine onto the remote machine

It is the latter of these two options that allows us to use this approach, as these machines are behind a NAT enabled network, and we have no access to the router which connects to the ISP - so we can't setup dynamic DNS style services.

## External Server

In order for us to tunnel into our local network, there needs to be an external server which acts as a gateway. A cheap and cheerful VPS will suffice for this. Let's look at what we will need to do:

### Enable port forwarding via SSH

Use mod_proxy to redirect specific subdomains to specific ports (we don't really want to be using a range of port numbers to access HTTP traffic, we want to use nice subdomains).

Point a wildcard subdomain to the server (For me, the subdomain is actually the server name so its a wildcard subdomain `*.servername.platformname.com`).

### SSH Settings
I'm using OpenSSH on Ubuntu for the Gateway server, there were some changes / additions needed in the config file (/etc/ssh/sshd_conf). Ensure the following setting value pairs are present:

    AllowTcpForwarding  yes
    X11Forwarding   yes
    GatewayPorts     yes
    AddressFamily   inet

Reload sshd:
`sudo /etc/init.d/sshd reload`
I've also setup my SSH server to work with keys, and setup keys for the team - so when we expose an Internal VM to the outside world, we can script it without the need for passwords.

### mod_proxy
The apache module mod_proxy lets us, among other things, map a request to a specific virtual host to a different port. This means for each VM connected to our gateway, each with a different port mapping - we can give them their own virtual host, without having to worry about which port is used by which machine.

#### Install
    sudo apt-get install libapache2-mod-proxy-html
    sudo apt-get install libxml2-dev
#### Enable
`sudo nano /etc/apache2/apache2.conf`

Add at the end:

    LoadModule proxy_module /usr/lib/apache2/modules/mod_proxy.so
    LoadModule proxy_http_module    /usr/lib/apache2/modules/mod_proxy_http.so
    LoadModule headers_module /usr/lib/apache2/modules/mod_headers.so
    LoadModule deflate_module /usr/lib/apache2/modules/mod_deflate.so

Reload apache to apply:
`sudo /etc/init.d/apache2 restart`

#### Configure virtual host
Edit your existing virtual host or create a new one.

    ServerAdmin email@domain.com
    ServerName sub.domain.platform.com
    ProxyPass / http://sub.domain.platform.com:10000/
    ProxyPassReverse / http://sub.domain.platform.com:10000/
    DirectoryIndex index.php
    DocumentRoot /var/www

If creating a new virtualhost, enable it: sudo a2ensite virtualhostname
Reload apache:
`sudo /etc/init.d/apache2 reload`

## Wildcard DNS
If you want multiple machines routed through, then you will probably want a wildcard subdomain setup at your DNS level.
## SSH with Remote Port Binding
The key to the whole operation.
From our internal, NAT enabled machine, we simply run the following command:
ssh user@gatewaymachine -R 10000:localhost:80
This will connect to our gateway machine, and set port 10000 on the gateway to map to port 80 on our specific internal VM.
### AutoSSH
Depending on your Internet connection, SSH may not be very reliable. There are some setting changes you can make to ensure the connection is maintained even when idle, however this does not help when the connection is severed. AutoSSH helps you maintain a continuous connection, reconnecting when it does disconnect.
`sudo apt-get install autossh`
Then use the ssh command as above, but with autossh in place of ssh.
You can now access your internal services from the outside world!

