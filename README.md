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

  
  
