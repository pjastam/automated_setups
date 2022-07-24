# `cloud-init` files

Collection of [cloud init](https://cloud-init.io/) files for adding user data to Digital Ocean droplets.

## Images

- [`analysis.yaml`](analysis.yaml): Installs R (with the hadleyverse and close friends) + Python 3 + numpy + pandas + Docker
- [`ovpn.yaml`](ovpn.yaml): Installs and configures an OpenVPN server and generates a ready-to-use client certificate
- [`shiny-server.yaml`](shiny-server.yaml): Installs R + Nginx + Shiny
