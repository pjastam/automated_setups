# Cloud-config scripts

The scripts in this repository are written in YAML format and can be used as user data when launching cloud instances on various cloud providers, such as Hetzner or DigitalOcean. The contents of the user data field are passed to the instance as a file, which is then processed by the cloud-init service. This allows you to automate the setup of your instance, such as installing software, configuring services, and running scripts. [Cloud init](https://cloud-init.io/) is the standard for customising cloud instances.

# Requirements

Check these things manually first:
- Make sure the R apt source matches the system
  (see https://cran.rstudio.com/bin/linux/ubuntu/fullREADME.html for names)
- If running on smaller droplet uncomment the section that enlarges swapfile
- Optional: add SSH public key for user account

## Scripts

You can find the scripts in the `scripts` directory, or click here:

- [`analysis.yaml`](scripts/analysis.yaml): Installs R (with the hadleyverse and close friends) + Python 3 + numpy + pandas + Docker
- [`openvpn.yaml`](scripts/openvpn.yaml): Installs and configures an OpenVPN server and generates a ready-to-use client certificate
- [`shiny-server.yaml`](scripts/shiny-server.yaml): Installs R + Nginx + Shiny
- [`pptx2md.yaml`](scripts/pptx2md.yaml): Installs pptx2md

## Documentation

You can find the explanations of the scripts in the documentation that is created with the help of Quarto and GitHub Pages. See the link in the About section of this repository for the documentation site.

## Sources

The original code was adapted from various sources, including the scripts in [this respository](https://github.com/andrewheiss/cloud-config-files/) by [Andrew Heiss](https://www.andrewheiss.com/) and [this tutorial](https://www.digitalocean.com/community/tutorials/an-introduction-to-cloud-config-scripting) by the [Digital Ocean](https://www.digitalocean.com/) community. Also, I benefited from the results of penetration tests that have been performed for acquiring ISO and NEN certifications for my consulting firm Equalis.
