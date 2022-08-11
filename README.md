# `cloud-init` files

Collection of [cloud-config scripts](https://www.digitalocean.com/community/tutorials/an-introduction-to-cloud-config-scripting) for [cloud init](https://cloud-init.io/) to execute by including them as user-data during the creation of [Digital Ocean](https://www.digitalocean.com/) droplets 

## Images

- [`analysis.yaml`](analysis.yaml): Installs R (with the hadleyverse and close friends) + Python 3 + numpy + pandas + Docker
- [`openvpn.yaml`](openvpn.yaml): Installs and configures an OpenVPN server and generates a ready-to-use client certificate
- [`shiny-server.yaml`](shiny-server.yaml): Installs R + Nginx + Shiny
