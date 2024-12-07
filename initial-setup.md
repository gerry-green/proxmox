- boot host using proxmox iso and follow instructions
- find the IP address assigned to the new node and reserve it in unifi
- login to the node via the pve dashboard and update the host name to add an ordinal (/etc/hosts and /etc/hostname)
- reboot the node to take effect
- add the no-subscription repositories and disable enterprise
- add the hostname / IP to cloudflare
- from the terminal, add authorized keys for my user to hit root
- run this script from the root shell on the new pve node - initializes a non-root user with sudo perms and zsh, etc.
````bash
#!/bin/bash
export user=gerry
# install several tools we will need
apt-get update && apt-get install -y sudo zsh curl git jq

# silence the fucking terminal bell
sed -iE '/^# set bell-style none/s/^# //' /etc/inputrc

# check if target user already exists
getent passwd | grep -q ^$user && { echo "user $user already exists dumbass"; exit 1; }

# add sudo group if not present
getent group | grep -q sudo || groupadd sudo

useradd $user
usermod -aG sudo $user
mkdir -p /home/$user/.ssh
grep $user ~/.ssh/authorized_keys > /home/$user/.ssh/authorized_keys
chmod 600 /home/$user/.ssh/*

echo "$user ALL=(ALL:ALL) NOPASSWD: ALL" > /etc/sudoers.d/$user-is-a-god

chsh -s $(which zsh) $user
touch /home/$user/.zshrc

curl -fsSL https://raw.githubusercontent.com/gerry-green/scripts/main/init-user > /home/$user/init-user
chmod +x /home/$user/init-user

chown -R $user:$user /home/$user

````
