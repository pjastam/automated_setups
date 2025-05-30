# Cloud-config scripts

Collection of [cloud-config scripts](https://www.digitalocean.com/community/tutorials/an-introduction-to-cloud-config-scripting) for the Ubuntu package [cloud-init](https://help.ubuntu.com/community/CloudInit) to execute by including them as [user-data](https://www.digitalocean.com/blog/automating-application-deployments-with-user-data) during the creation of [Digital Ocean](https://www.digitalocean.com/) droplets. [Cloud init](https://cloud-init.io/) is the standard for customising cloud instances.

## Images

- [`analysis.yaml`](analysis.yaml): Installs R (with the hadleyverse and close friends) + Python 3 + numpy + pandas + Docker
- [`openvpn.yaml`](openvpn.yaml): Installs and configures an OpenVPN server and generates a ready-to-use client certificate
- [`shiny-server.yaml`](shiny-server.yaml): Installs R + Nginx + Shiny
- [`pptx2md.yaml`](pptx2md.yaml): Installs pptx2md

## Explanations

### analysis.yaml

Source: https://github.com/andrewheiss/cloud-config-files

Creates a new user named "piet" with password-less sudo capabilities
SSH is available on port 4444

Log is saved as /var/log/cloud-init-output.log

Check these things manually first:
- Make sure the R apt source matches the system
  (see https://cran.rstudio.com/bin/linux/ubuntu/fullREADME.html for names)
- If running on smaller droplet uncomment the section that enlarges swapfile
- Optional: add SSH public key for user account

Help on cloud-init syntax from:
- https://cloudinit.readthedocs.io/en/latest/topics/examples.html
- https://www.digitalocean.com/community/tutorials/how-to-use-cloud-config-for-your-initial-server-setup
  (ERROR: THE ssh-authorized-keys (AND sudo) SYNTAX IS WRONG **IN ALL DIGITAL OCEAN EXAMPLES**, SEE NEXT REFERENCE)
- https://superuser.com/questions/1725127/invalid-config-for-cloud-init-but-apparently-still-works-how-do-i-remove-the

Help on R and Docker installation syntax from:
- http://deanattali.com/2015/05/09/setup-rstudio-shiny-server-digital-ocean/
- https://www.digitalocean.com/community/tutorials/how-to-set-up-r-on-ubuntu-14-04
- https://gist.github.com/mdlincoln/1f40f4e88ce32c55b5f3
- https://gist.github.com/mdlincoln/1f40f4e88ce32c55b5f3#gistcomment-3009981
- https://cran.rstudio.com/bin/linux/ubuntu/fullREADME.html#using-apt-key
- https://github.com/rocker-org/hadleyverse/blob/master/Dockerfile
- https://stackoverflow.com/questions/24418815/how-do-i-install-docker-using-cloud-init

### openvpn.yaml

Source:
- https://github.com/andrewheiss/cloud-config-files/ovpn.yaml
- https://github.com/Nyr/openvpn-install

Creates a new user named "vpn" with password-less sudo capabilities
SSH is available on port 4444

Log is saved as /var/log/cloud-init-output.log

Check these things manually first:
- Optional: add SSH key for user account

OpenVPN is configured automatically and the client certificate is available at /home/vpn/client.ovpn
at your newly installed OpenVPN server. Then follow these steps:
1. Transfer this ovpn file to your client (Windows: C:\Users\<USERNAME>\OpenVPN\config) using scp like so:
       scp -P 4444 vpn@IPADDRESS:/home/vpn/client.ovpn client.ovpn
2. Install the OpenVPN client.
3. Run the OpenVPN client (see the system tray in Windows).
4. Click yes if asked to add yourself to the "OpenVPN Administrators" group (administrator password required) 

Help on cloud-init syntax from:
- https://cloudinit.readthedocs.io/en/latest/topics/examples.html
- https://www.digitalocean.com/community/tutorials/how-to-use-cloud-config-for-your-initial-server-setup
  (ERROR: THE ssh-authorized-keys (AND sudo) SYNTAX IS WRONG **IN ALL DIGITAL OCEAN EXAMPLES**, SEE NEXT REFERENCE)
- https://superuser.com/questions/1725127/invalid-config-for-cloud-init-but-apparently-still-works-how-do-i-remove-the

### pptx2md.yaml

Use this file to create an Ubuntu server, install pptx2md and convert a Powerpoint pptx file into markdown
After the install, create a subdirectory called tmp, chdir tmp, upload the pptx file and excute the following cmd:
/usr/bin/python3.8 /usr/local/bin/pptx2md filename.pptx

PREREQUISITES:
- python 3.8 or 3.9 (not 3.10, see issue https://github.com/scanny/python-pptx/issues/770)
- execute pptx2md as a root (see issue https://github.com/ssine/pptx2md/issues/25)

REFERENCES:
- The tool to convert a Powerpoint pptx file into markdown: https://github.com/ssine/pptx2md
- To find the Python versions that are installed by default: whereis python
- How to Install and Switch Python Versions on Ubuntu 20.04: https://www.rosehosting.com/blog/how-to-install-and-switch-python-versions-on-ubuntu-20-04/

### shiny-server.yaml

Set up a Shiny server on Ubuntu 20.04 - without SSL certificate
- see https://cloud.r-project.org/bin/linux/ubuntu/ for the choice of the Ubuntu version
- also check the shiny server version number at https://www.rstudio.com/products/shiny/download-server/ubuntu/

Creates a new user named "piet" with password-less sudo capabilities
SSH is available on port 4444

Log is saved as /var/log/cloud-init-output.log

Check these things manually first:
- Make sure the R apt source matches the system
  (see https://cran.rstudio.com/bin/linux/ubuntu/fullREADME.html for names)
- If running on smaller droplet uncomment the section that enlarges swapfile
- Optional: add SSH public key for user account

Some checks to do after the cloud-init script has finished:
- (SSH) Is SSH key added? sudo nano ~/.ssh/authorized_keys
- (SSH) Has port changed from 22 to 4444? sudo nano /etc/ssh/sshd_config
- (Nginx) Is the content of the server block written to the file that is included in the Nginx config file /etc/nginx/conf.d? sudo nano /etc/nginx/sites-enabled/default
- Are .deb files deleted? sudo ls -l /var/cache/apt/archives
  - If not, apply sudo apt-get clean (source: https://www.jverdeyen.be/ubuntu/digital-ocean-ubuntu-free-up-disk-space/)
- (sed) Check sed syntax at https://sed.js.org/index.html, this tool was used to solve the issue with port 4444 (see commit w/ hash fcb426dd536952ce929329a865d737660d12f112)

Help on cloud-init syntax from:
- https://cloudinit.readthedocs.io/en/latest/topics/examples.html
- https://www.digitalocean.com/community/tutorials/how-to-use-cloud-config-for-your-initial-server-setup
  (ERROR: THE ssh-authorized-keys (AND sudo) SYNTAX IS WRONG **IN ALL DIGITAL OCEAN EXAMPLES**, SEE NEXT REFERENCE)
- https://superuser.com/questions/1725127/invalid-config-for-cloud-init-but-apparently-still-works-how-do-i-remove-the
- https://github.com/andrewheiss/cloud-config-files

Help on Shiny server installation syntax from:
- http://deanattali.com/2015/05/09/setup-rstudio-shiny-server-digital-ocean/
- https://www.digitalocean.com/community/tutorials/how-to-set-up-shiny-server-on-ubuntu-20-04
- https://gist.github.com/mdlincoln/1f40f4e88ce32c55b5f3
- https://gist.github.com/mdlincoln/1f40f4e88ce32c55b5f3#gistcomment-3009981
- https://cran.rstudio.com/bin/linux/ubuntu/fullREADME.html#using-apt-key
