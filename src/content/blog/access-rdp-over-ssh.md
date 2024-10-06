---
title: How to Access RDP over an SSH Tunnel
author: recluzegeek
pubDatetime: 2024-07-21T10:11:06.130Z
modDatetime: 2024-10-06T17:30:23Z
slug: how-to-access-rdp-over-ssh
featured: true
draft: false
_canonicalURL_: https://eviatargerzi.medium.com/how-to-access-rdp-over-ssh-tunnel-c0829631ad44
description: Access your windows machine over the internet
tags:
  - tutorial
  - tunnel
  - rdp
---

## Table of contents

## Introduction

The Windows RDP (Remote Desktop Protocol) tool allows you to remotely access machines that support the RDP protocol. Most Windows machines come with RDP enabled by default, requiring little to no configuration. However, on Unix-based systems, some additional setup is necessary to get things working. Once everything is configured, you can access machines within your local area network (LAN). If you're having trouble, ensure that both machines are connected to the same network to enable communication.

## Problem Statement

Accessing machines on the same network is straightforward, but people often become accustomed to remote access—even to machines in another room or a home lab—while lounging in their bedroom. For frequent travelers needing access to their home machines, ngrok can create a secure tunnel for you. Follow these steps to set it up:

## NGROK

1. Download and install ngrok from the official site. If you're using Windows, make sure to add the ngrok executable to your Environment Variables.
2. Go to the [NGROK Dashboard](https://dashboard.ngrok.com) and copy your authentication code.
3. Paste the auth-token into your terminal:

    ```bash
    ngrok config add-authtoken TOKEN
    ```

4. Run the following command in your terminal:

    ```bash
    ngrok tcp 3389
    ```

This command will provide you with a public URL that allows you to access your computer from anywhere in the world.

### Cons of NGROK

While ngrok is convenient, it is not free and operates on a subscription model. The free tier has a traffic limit of 1GB, which can be quickly consumed if you use it frequently. So, what are the alternatives for accessing machines over the internet?

> One option is SSH tunneling, but it requires a server with a public IP address. Once you've a public server instance running; creating tunnel is easier than it sounds!

## SSH Tunneling

SSH tunneling is a reliable solution for remote accessing. We can make use of SSH tunneling to facilitate  bidirectional communication between computers via a middleman which is a server - a public instance.

**Machine A represents your university, work, or lab machine that you want to access from Machine B, which is your personal machine.**

For simplicity, we'll be using `plink.exe` utility which comes with installing **putty**. The command to set it up is as follows:

```bash
plink.exe <user>@<ip or domain> -pw <password> -P 22 -2 -4 -T -N -C -R 0.0.0.0:12345:127.0.0.1:3389
```

 This command will be executed on Machine A from which you wish to gain remote access.

### Explanation of the Flags

This guide assumes you've set up password authentication with SSH; if you're using public/private key authentication, adjust accordingly by passing path to private key with `-i` flag.

- `plink.exe`: This is the command-line interface to PuTTY, used for establishing SSH connections.
- `<user>`: Your username for the remote machine.
- `<ip or domain>`: The IP address or domain name of the remote machine.
- `-pw <password>`: Specifies the password for the user.
- `-P 22`: Sets the port number for SSH (22 is the default).
- `-2`: Forces the use of SSH protocol version 2.
- `-4`: Forces the use of IPv4.
- `-T`: Disables pseudo-terminal allocation, which is useful for commands that don’t require a terminal.
- `-N`: Indicates that no command will be executed on the remote machine; this is often used for port forwarding.
- `-C`: Enables compression, which can speed up the connection.
- `-R 0.0.0.0:12345:127.0.0.1:3389`: Sets up remote port forwarding. It makes the local RDP service (port 3389) accessible on port 12345 of the remote machine, allowing you to connect from the internet.

This setup provides a secure method to access your machines, even when you're away from home.
