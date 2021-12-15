# Zenon-Pillar-Sentry-Architecture-For-Running-Pillars

### Background
Pillars are an integral part of the network so it is critical that we treat them as such and take measures to reduce their attack surface. One way to do this is to set up a sentry architecture. In it's simplest terms that means we will remove the list of peers that are populated when you spawn your pillar and replace them with your own personal sentry nodes. We will also adjust the firewall so that your pillar is communicating only with your sentry nodes. In this instance if someone was to try to DDOS you the best they could do is take your sentry offline. Assuming you have multiple sentries your pillar will still be able to communicate with them until you mitigate the attack. 

### Prereqs
1. You will need at least 2 sentry nodes to support your pillar (more is fine). It is recommended to launch these sentry nodes across regions / availability zones to protect yourself against unexpected outages.
2. You will need to have a running pillar. If you are launching a pillar for the first time refer to the teams pillar launching guide https://github.com/zenon-network/znn-bundle/blob/master/PILLARS.md or SultanOfStaking guide https://github.com/sultanofstaking/Zenon-Pillar-Deployment.

### Steps to Sentrify an existing pillar

There are 2 options to sentrify your pillar. Option 1 is to install dart and compile the code yourself. Option 2 is the download the pre-compiled binary from github. This guide covers Option 2 as it is few steps and easier for most without a technical background.

***Start from Root***

### Get sentrify and make executable

`wget https://github.com/MoonBaZe/sentrify/releases/download/release/sentrify
chmod +x sentrify`

### Stop znn-controller service (Note, this will stop producing momentum so once you do this you will want to finsh the setup quickly)

`sudo systemctl stop go-zenon.service`
 
### Remove network folder

`rm -rf /root/.znn/network`

### Remove peers

`./sentrify` and choose option `3` - this will remove all peers from config.json 

### Add sentries

`./sentrify` and choose option `2` - this will ask for sentry ip and then ws port (copy paste the default port), so it can take it's public key and add it repeat for all your sentries

Note, if you receive "Can not establish a connection with the sentry" then check the firewall on your sentry `sudo ufw status` and add the ip of your pillar if needed `ufw allow from YOUR IP`

### Sentrify

`./sentrify` and choose option `4` - will add firewall rules so that your node only accepts ssh, dns (so controller works) and your sentries

### Enable firewall

`ufw enable` - then `y` - don't worry, will not interrupt your ssh

### Start node

`sudo systemctl start go-zenon.service`

### Check status

`systemctl status go-zenon.service`

### Tips

Every time you add a new sentry (step `2` in `./sentrify`, you should run `./sentrify` option `4` and then `enable ufw`
