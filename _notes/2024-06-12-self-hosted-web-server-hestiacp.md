---
title: "Self-Hosted Web Server with HestiaCP"
date: 2024-06-12
excerpt: "A quick, repeatable setup guide for a self-managed web server using HestiaCP on Ubuntu or Debian."
tags:
  - hestiacp
  - vps
  - cloudflare
---

I recently moved to [HestiaCP](https://hestiacp.com/) from [VestaCP](https://www.vestacp.com/) because VestaCP stalled on updates and progress.

HestiaCP is a fork of VestaCP and gives you a clean panel, sane defaults, and a straightforward path to mail, SSL, and web services without piecing everything together manually. I use it for my personal web server and side projects — it’s cheaper to run this way, and I typically spin up Linux VMs on DigitalOcean.

These are the standard steps I follow to get the control panel up and running.

## Spin up a latest Ubuntu or Debian based VM or Droplet

Read supported versions and system requirements:
https://hestiacp.com/docs/introduction/getting-started.html

## Locale setup

```bash
locale-gen en_US.UTF-8
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
```

## Install HestiaCP using Standard Script

https://hestiacp.com/docs/introduction/getting-started.html

```bash
wget https://raw.githubusercontent.com/hestiacp/hestiacp/release/install/hst-install.sh

# If the download fails due to an SSL validation error
apt-get update && apt-get install ca-certificates

bash hst-install.sh
```

## Change the panel port to 2083 to support Cloudflare DNS proxy

### Method 1

```bash
cd /usr/local/hestia/nginx/conf/
```

### Method 2

```bash
v-change-sys-port 2083
```

## Add cert to admin user for Cloudflare SSL Strict Mode to work

Run the following command as root user.

```bash
v-add-letsencrypt-host
```

## Notes

- If you are using DigitalOcean and your DNS points to a Floating IP, resolution won't work.
