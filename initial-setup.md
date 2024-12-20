- boot host using proxmox iso and follow instructions
- find the IP address assigned to the new node and reserve it in unifi
- login to the node via the pve dashboard and update the host name to add an ordinal (/etc/hosts and /etc/hostname)
- reboot the node to take effect
- add the no-subscription repositories and disable enterprise
- add the hostname / IP to cloudflare
- from the terminal, add public key to /root/.ssh/authorized_keys for the user
- copy the github private key to /root/.ssh
- run this script from the root shell on the new pve node - initializes a non-root user with sudo perms and zsh, etc.
- todo: update script to a one-liner using curl
````bash
wget https://raw.githubusercontent.com/gerry-green/proxmox/main/init/init-node
chmod +x init-node
./init-node gerry
````
## User setup
- in the proxmox console / datacenter create a group "admin"
- add a permission to the group admin for the root of the data center and Administrator (the top level 'Permissions' tab is not just a folder but also facilitates managing persmissions too)
- create a user in the proxmox console and put it in the admin group 