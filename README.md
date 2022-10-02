# Protection-and-notification-system-for-validators-HAQQ-Islamic-Coin-

Tenderduty is a comprehensive monitoring tool for Tendermint networks.
This monitoring of TenderDuty v2 allows you to control the nodes and, in particular, see the height of the network, the status of the validator, uptime, signed and skipped blocks. It is also possible to connect notifications to telegrams and discord
Installation is possible in various ways, but I will use installation via Docker, although there is no fundamental difference
So, we need a separate server (which definitely gives a security plus) or a server with an already installed node (nodes). You will also need to find open RPCs or open your own on the main (not desirable) or backup node
Server preparation

# Server preparation
 update repositories
 
       sudo apt update && sudo apt upgrade -y

 install the necessary utilities
 
       sudo apt install curl build-essential git wget jq make gcc tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y

 install and copy the config, which will have a higher priority
 
      apt install fail2ban -y && \
      cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local && \
      nano /etc/fail2ban/jail.local
      
 uncomment and add your IP: ignoreip = 127.0.0.1/8 ::1 <ip>
  
      systemctl restart fail2ban
  
 check status
  
      systemctl status fail2ban
  
 check which jails are active (only sshd by default)
  
      fail2ban-client status
  
 check sshd statistics
  
      fail2ban-client status sshd
  
 look at the logs
  
      tail /var/log/fail2ban.log
  
 stop work and remove from startup
  
      systemctl stop fail2ban && systemctl disable fail2ban
  
# docker installation

# https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04-en
    apt update && \
    apt install apt-transport-https ca-certificates curl software-properties-common -y && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && \
    apt update && \
    apt-cache policy docker-ce && \
    sudo apt install docker-ce -y && \
    docker --version

# Setting
Install tenderduty
    tmux new-session -s tenderduty

    mkdir tenderduty && cd tenderduty
    docker run --rm ghcr.io/blockpane/tenderduty:latest -example-config >config.yml
  
Now you can download the config and edit it
  
    wget -O $HOME/tenderduty/config.yml "https://github.com/AndreyT25/Protection-and-notification-system-for-validators-HAQQ-Islamic-Coin-/blob/main/config.yml"
    nano $HOME/tenderduty/config.yml
For simple monitoring without notifications, just change in the config:
- network name:
- chain-id:
- valoper_address:
- url:  
# Example config file

  https://github.com/AndreyT25/Protection-and-notification-system-for-validators-HAQQ-Islamic-Coin-/blob/main/config.yml
---

# After setting up the config, run

docker run -d --name tenderduty -p "8888:8888" -p "28686:28686" --restart unless-stopped -v $(pwd)/config.yml:/var/lib/tenderduty/config.yml ghcr .io/blockpane/tenderduty:latest

# look at the logs
docker logs -f --tail 20 tenderduty

Now is the time to check the information in the browser

# find out the address and paste it into the browser
echo -e "\033[0;32mhttp://$(wget -qO- eth0.me):8888/\033[0m"
# http://108.108.108.108:8888/


Discord setup
It's pretty easy to turn on notifications for Discord. To do this, you need to perform just a few steps
Adding a server
