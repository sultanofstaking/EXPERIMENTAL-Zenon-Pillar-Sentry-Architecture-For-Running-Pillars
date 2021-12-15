# Zenon-Pillar-Sentry-Architecture

## Node Requirements
Hardware, CPU >= 4 cores, RAM >= 8 GB, Swap >= 16 GB, Storage >= 40 GB free space, >100Mbps network dedicated bandwidth Software, Linux distros e.g. Ubuntu 20.04 LTS/Debian 11 (recommend ubuntu 20.04)

Also Recommended NTP configuration*, Recommended Watchdog service* These are Included if the setup is performed using the znn-controller

### Prereqs
1. You will need at least 2 sentry nodes to support your pillar (more is fine). It is recommended to launch these sentry nodes across regions / availability zones because to protect yourself against unexpected outages.
2. You will need to have a running pillar. If you are launching a pillar for the first time refer to the teams pillar launching guide https://github.com/zenon-network/znn-bundle/blob/master/PILLARS.md or SultanOfStaking guide https://github.com/sultanofstaking/Zenon-Pillar-Deployment.

### Background
Pillars are an integral part of the network so it is critical that we treat them as such and take measures to reduce their attack surface. One way to do this is to set up a sentry architecture. In it's simplest terms that means we will remove the list of peers that are populated when you spawn your pillar and replace them with your own sentry nodes that you run. We will also adjust the firewall so that your pillar is communicating only with your sentry nodes. In this instance if someone was to try to DDOS you the best they could do is take your sentry offline. Assuming you have multiple sentries your pillar will still be able to communicate with them until you mitigate the attack. 

### Steps to Sentrify an existing pillar
Start from Root

Get Dart SDK???
Need to create packages directory? Root mkdir packages
Compile in pillar concern? run in packages directory? OR transfer to /root/packages


Download process (link is unique to OS and Architecture - this is for Linux x64)

```
cd packages
sudo curl -sL https://storage.googleapis.com/dart-archive/channels/stable/release/2.14.2/sdk/dartsdk-linux-x64-release.zip -o sentrify
sudo chmod +x sentrify
sudo unzip sentrify
```


Create and navigate to alphanet directory
`mkdir alphanet && cd alphanet`

Get sentrify and make executable
`wget https://github.com/MoonBaZe/sentrify/releases/download/release/sentrify
chmod +x sentrify`

Stop znn-controller service (this will stop producing momentum)
`sudo systemctl stop go-zenon.service`
 
Remove peers
`./sentrify` and choose option `3` - this will remove all peers from config.json if you have any

Add sentries
`./sentrify` and choose option `2` - will ask for sentry ip and then ws port, so it can take it's public key and add it repeat for all your sentries

Sentrify
`./sentrify` and choose option `4` - will add firewall rules so that your node only accepts ssh, dns (so controller works) and your sentries

Enable firewall
`ufw enable` - then `y` - don't worry, will not interrupt your ssh

Start node
`sudo systemctl start go-zenon.service`

Check status
`systemctl status go-zenon.service`

Tips
Every time you add a new sentry (step `2` in `./sentrify`, you should run `./sentrify` option `4` and then `enable ufw`
