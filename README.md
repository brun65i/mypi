# My Raspberry PI home server

This is an Ansible playbook to configure Raspberry PI home server.
Please note that I am using 32-bit Raspberry Pi OS, so all images I am using are armhf

All services are running in Docker. The following services are running so far

* seprate role to install **docker-ce**
* **pihole** with **dhcp-helper** wich makes it is possible to run DHCP server inside docker
* **watchtower** to keep all containers up to date

There is an issue with pihole service. You have to enable DHCP server manually after first run.