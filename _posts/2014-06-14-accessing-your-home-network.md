---
layout: post
title: "Using SSH and OpenWrt to access your home network from the outside"
description: "Step by step guide to access your home network from outside"
modified: 2014-06-14
category: articles
tags: general
---


I recently set up access to my home network so that I can access it when I am not home. I could not find a tutorial on the web so I thought I should write one.

This guide is specifically for Linux and OpenWrt users, but you can probably do something similar in other environments too.

##### Pre-requisites:

- A router with [OpenWrt][openwrt] installed (with dropbear)
- A linux machine at home (with sshd)
- Another linux machine to test your setup (with ssh client)
- Key based ssh access to the router and the home machine

# Step 1: Configure OpenWrt

SSH into your OpenWrt router and enable port forwarding by adding the following block to `/etc/config/firewall`:

<pre>
config redirect
    option src wan
    option src_dport 10022
    option dest_port 22
    option proto tcp
    option dest lan
    option dest_ip 192.168.1.22
</pre>

After updating the file, execute this to enable the new firewall rules:

<pre>
/etc/init.d/firewall restart
</pre>

The above configuration forwards all traffic coming in on port *10022* on the router to the internal IP address *192.168.1.22*.

`src` is the network from which packets will be redirected

`src_dport` is the port on which the router will listen on publicly

`dest_port` is the port exposed on your home machine

`dest` is the network on which openwrt will route the packets to

`dest_ip` is the IP address of your home computer


If your router is directly connected to the internet (or via a cable modem), you should now be able to access port `10022` from the outside.

You can verify this by ssh'ing into your machine using the public IP:

<pre>ssh $PUBLIC_IP_OF_HOME -p 10022</pre>

If this works, please proceed to *Step 2*, otherwise make sure that sshd is running on your home machine, and that your router is directly connected to the internet.

# Step 2: Add your host to ssh_config
It is not convenient to type the ssh command with an IP address, port etc all the time, so lets create an entry in ssh_config to simplify things.

Add this to your `~/.ssh/config`:
<pre>
Host home.public
IdentityFile $PATH_TO_YOUR_PRIV_KEY
User $YOUR_USER
Port 10022
ForwardAgent yes
Hostname $PUBLIC_IP_OF_HOME
</pre>

If you would like to know more about the configuration options, please read the man page for ssh_config. Now you can ssh into your home machine by doing this:
<pre>
ssh home.public
</pre>


# Step 3: Access your home services in a secure way
The SSH client can do port forwarding to transparently access services running on a remote machine. This means that we can access services running on the home machine in a secure way. Just add this in `~/.ssh/config` (to the block we created in Step 2):
<pre>
LocalForward 8888 localhost:80
</pre>

This will forward port 8888 on your machine to port 80 of your home machine.

# Step 4: Keep track of your public IP
If you don't have the privilege of a static IP address, you would need to keep track of it since it normally changes every once in a while. There are two options to solve this:

1. Use something like [dyndns][dyndns], [noip][noip] etc. Both have an HTTP API to update the IP, so basically you just need to send an HTTP call (using curl or something) every one minute (using cron) to update your IP address. If you would like to know your current public IP address, just do: `curl ifconfig.me`. Kudos to whoever runs that site.

2. Roll your own. If you don't want to use a dynamic dns service, you can periodically send an HTTP request to a remote server with your IP address in the path. Just install nginx (or Apache) on your remote server and enable access logging. No need to even write an application to store the IP address in a database. Just tail the access logs and see what your home public IP address is. The downside of this is that whenever your home IP changes, you would need to login to your remote server to find out the new IP. That's not too bad!


I hope this was useful.


[openwrt]: https://openwrt.org/
[dyndns]: https://dyndns.com
[noip]: http://noip.com
