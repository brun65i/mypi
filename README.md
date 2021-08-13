# My Raspberry PI home server

This is an Ansible playbook to configure Raspberry PI home server.
Please note that I am using 32-bit Raspberry Pi OS, so all images I am using are armhf

All services are running in Docker. The following services are running so far

* seprate role to install **docker-ce**
* **pihole** with **dhcp-helper** wich makes it is possible to run DHCP server inside docker
* **watchtower** to keep all containers up to date

## PiHole

There are still two issues in a playbook presert.
 -  "Install Docker-CE" task is failing as it is not able to start docker service. Reboot and try again.
 -  "Create a network with custom IPv4 IPAM config" is not able to crate the network as **pi** user was just added to docker group, so we need to refresh ssh connection to fix this.

 Please update **roles/pihole/vars/main.yml** with your network subnet settings.