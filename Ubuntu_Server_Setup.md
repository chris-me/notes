### Links

* https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04
* https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-16-04
* https://www.digitalocean.com/community/tutorials/an-introduction-to-securing-your-linux-vps
* https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04

### Steps

```bash
sudo dpkg-reconfigure tzdata
vi /etc/vim/vimrc # background dark
adduser USERNAME
gpasswd -a USERNAME sudo
#open new session as new user

#SSH
sudo vi /etc/ssh/sshd_config
# change SSH port
# PermitRootLogin no
sudo systemctl restart ssh

#UFW
sudo ufw allow 20022/tcp
sudo ufw enable
sudo ufw status

#SWAP
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
ls -lh /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo swapon --show
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

#System Upgrade
sudo apt update
sudo apt full-upgrade
sudo reboot
sudo apt autoremove
```
