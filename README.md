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
  
    wget -O $HOME/tenderduty/config.yml "https://raw.githubusercontent.com/AndreyT25/Protection-and-notification-system-for-validators-HAQQ-Islamic-Coin-/main/config.yml"
    nano $HOME/tenderduty/config.yml

For simple monitoring without notifications, just change in the config:
- network name: haqq
- chain-id: haqq_54211–2
- valoper_address: Replace with your address
- url: http://142.132.202.50:11601/ 
# Example <a href="https://github.com/AndreyT25/Protection-and-notification-system-for-validators-HAQQ-Islamic-Coin-/blob/main/config.yml"> config.yml </a> file

---
# Discord setup
 
It's pretty easy to turn on notifications for Discord. To do this, you need to perform just a few steps
Adding a server
 
 ![1](https://user-images.githubusercontent.com/95170659/193444982-0be9e722-f62b-4004-8751-275a228a6216.png)
 
Choose your template
 
 ![2](https://user-images.githubusercontent.com/95170659/193445062-6ffa9d24-8930-487b-baa3-55e78a83bb93.png)
 ![3](https://user-images.githubusercontent.com/95170659/193445110-c24f3256-07a0-49f1-87da-3a3d9938b49e.png)
 ![4](https://user-images.githubusercontent.com/95170659/193445115-3ad0c37a-c937-4e6c-aa67-d0412d1e47db.png)
 ![5](https://user-images.githubusercontent.com/95170659/193445145-68fea2c3-4146-423e-8df6-7317f2860020.png)
 
Then we go to integration and create a Webhook
 
 ![6](https://user-images.githubusercontent.com/95170659/193445158-3c04f976-168e-470e-afc8-db2a6ed190a9.png)
 
Copy the webhook link, which we paste into the monitoring config
 
 ![7](https://user-images.githubusercontent.com/95170659/193445242-7197972e-de86-40b7-9683-1823db3c5e90.png)
 
Change discord settings in config.yml
 - discord:
- Notifications in discord?
- enabled: yes
- The webhook is set up by right clicking on the channel, editing the settings and configuring the webhook in the integration section.
- webhook: https://discord.com/api/webhooks/999999999999999999/zzzzzzzz (Insert your webhook)

# After setting up the config, run

     docker run -d --name tenderduty -p "8888:8888" -p "28686:28686" --restart unless-stopped -v $(pwd)/config.yml:/var/lib/tenderduty/config.yml ghcr.io/blockpane/tenderduty:latest

# look at the logs
 
     docker logs -f --tail 20 tenderduty

Now is the time to check the information in the browser

# find out the address and paste it into the browser
 
     echo -e "\033[0;32mhttp://$(wget -qO- eth0.me):8888/\033[0m"



