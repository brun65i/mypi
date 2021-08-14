# My Raspberry PI home server

This is an Ansible playbook to configure Raspberry PI home server.
Please note that I am using 32-bit Raspberry Pi OS, so all images I am using are armhf

All services are running in Docker. The following services are running so far

* seprate role to install **docker-ce**
* **pihole** with **dhcp-helper** wich makes it is possible to run DHCP server inside docker
* **watchtower** to keep all containers up to date

## Initial setup

* To enabel ssh service `touch ssh` in boot volume
* Default credentials: pi raspberry
* Switch to static IP `nano /etc/dhcpcd.conf`
* `sudo raspi-config` to update locale and time zone

## PiHole

There are still two issues in a playbook presert.
 -  "Install Docker-CE" task is failing as it is not able to start docker service. Reboot and try again.
 -  "Create a network with custom IPv4 IPAM config" is not able to crate the network as **pi** user was just added to docker group, so we need to refresh ssh connection to fix this.

 Please update **roles/pihole/vars/main.yml** with your network subnet settings.
 Please update piHole password by executong the following command inside pihole container.
  ```bash
  sudo pihole -a -p
  ```

The following still needs to be implemented in the playbook.
**The Block List Project** is being used for extra ADS lists https://github.com/blocklistproject/Lists
```bash
# The only way to add blocklists via CLI is to add it to sqlite database
sudo sqlite3 /home/pi/containers/pihole/etc-pihole/gravity.db "INSERT OR IGNORE INTO adlist (address, enabled, comment) VALUES ('https://blocklistproject.github.io/Lists/porn.txt', 1, 'PORN');"
sudo sqlite3 /home/pi/containers/pihole/etc-pihole/gravity.db "INSERT OR IGNORE INTO adlist (address, enabled, comment) VALUES ('https://blocklistproject.github.io/Lists/ads.txt', 1, 'ADS');"
sudo sqlite3 /home/pi/containers/pihole/etc-pihole/gravity.db "INSERT OR IGNORE INTO adlist (address, enabled, comment) VALUES ('https://blocklistproject.github.io/Lists/tracking.txt', 1, 'TRACKING');"

# To download new lists we need to update gravity list.
docker exec pihole pihole -g
```