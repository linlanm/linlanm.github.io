---
title: Cloudflare WARP packages
date: 2023-10-27 13:52:57
tags:
- Internet
categories: 
- Internet
---
Cloudflare's client-side software can be installed on Linux with package managers APT or YUM by following these instructions. However, keep in mind that not all packages may support all operating systems or architectures and that you can check a specific package's page (linked from the homepage) to see what's available. We generally support modern versions of the following distributions:

Ubuntu
The supported releases are:
Jammy (22.04)
Focal (20.04)
Bionic (18.04)
Xenial (16.04)
# Add cloudflare gpg key
curl https://pkg.cloudflareclient.com/pubkey.gpg | sudo gpg --yes --dearmor --output /usr/share/keyrings/cloudflare-warp-archive-keyring.gpg


# Add this repo to your apt repositories
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/cloudflare-warp-archive-keyring.gpg] https://pkg.cloudflareclient.com/ $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/cloudflare-client.list


# Install
sudo apt-get update && sudo apt-get install cloudflare-warp