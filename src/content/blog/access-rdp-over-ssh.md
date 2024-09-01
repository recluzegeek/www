---
title: How to Access RDP over SSH Tunnel
author: M. Salman
pubDatetime: 2024-07-21T10:11:06.130Z
slug: how-to-access-rdp-over-ssh
featured: true
draft: false
tags:
  - rdp
  - tunnel
description: Access your windows machine over the internet?
---

## Table of contents

## Intro

Windows RDP tool is used to access machines remotely that supports RDP protocol. Most windows machine supports it by default with no configuration needed, while on unix based system we've to do little bit of extra work to make things working. When everything is setup, you can access the machines in your local area network. If things aren't working for you, make sure both machines are on the same network, so both can communicate.

## Problem Statement

Accessing machines on same network is a breeze, but its an addiction or you can say people are lazy to access machines that're located in other room, home lab and you're accessing them by sitting in your bedroom. If you're a frequent traveler and needs to access your home machine, you can make use of ngrok to make secure tunnel for you. Here are the steps to follow:

## NGROK

1. Download & Install ngrok from official site and add the ngrok executable to Environment Variables if you're on windows.
2. Visit [NGROK Dashboard](https://dashboard.ngrok.com) and copy your auth-code
3. Paste in the auth-token to your terminal

    ```bash
    ngrok config add-authtoken TOKEN
    ```

4. Run from the terminal

    ```bash
    ngrok tcp 3389
    ```

This will give you a public URL to your computer that is accessible from internet & you can access your system from anywhere in the world.

### Cons of NGROK

But NGROK is not FOSS, its subscription based service and has 1GB traffic which can run out very quickly if you're a frequent user like me. So, what're other ways to access the machine over the internet. One of them is SSH tunneling, but it needs a server with a public IP. Don't freak out, its easy to setup.

## SSH Tunneling

SSH tunneling comes to rescue...
