- boot host using proxmox iso and follow instructions
- find the IP address assigned to the new node and reserve it in unifi
- login to the node via the pve dashboard and update the host name (/etc/hosts and /etc/hostname)
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
getent passwd | grep -q ^$user && { echo user $user already exists dumbass; exit 1; }

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
touch /home/$user/initzsh
chmod +x /home/$user/initzsh

chown -R $user:$user /home/$user

cat <<EOF > /home/$user/initzsh
#!/usr/bin/zsh
cd /home/$user

# silence vim: (seriously who the fuck would want the bell on by default?)
echo "set belloff=all" > .vimrc
# and stop fucking with the line endings:
echo "set nofixeol" >> .vimrc

export RUNZSH=no
sh -c "\$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# install powerlevel10k theme and switch to it:
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git /home/$user/.oh-my-zsh/custom/themes/powerlevel10k
sed -i '/^ZSH_THEME=/s/=.*/=powerlevel10k\/powerlevel10k/' .zshrc

EOF

````
