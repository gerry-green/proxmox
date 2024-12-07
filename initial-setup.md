- boot host using proxmox iso and follow instructions
- find the IP address assigned to the new node and reserve it in unifi
- login to the node via the pve dashboard and update the host name to add an ordinal (/etc/hosts and /etc/hostname)
- reboot the node to take effect
- add the no-subscription repositories and disable enterprise
- add the hostname / IP to cloudflare
- from the terminal, add authorized keys for my user to hit root
- run this script from the root shell on the new pve node - initializes a non-root user with sudo perms and zsh, etc.
````bash
wget https://raw.githubusercontent.com/gerry-green/proxmox/main/init/init-node
chmod +x init-node
./init-node gerry
````
