# Zenon-Pillar-Sentry-Architecture

### Prereqs
Need 2 full nodes

### Background

Zenon uses port 35995 for node to node network communication.
What this means is that if you run a node, your computer is Listening on port 35995 for incoming connections. In order to connect with another computer's port 35995, you open up a temporary/ephemeral port on your computer to connect with their 35995.

### Steps
Try running
`netstat -a | grep 35995`
on your pillar/node. This will let you see everyone who has connected with your port 35995
and also let you see all your outbound connections with someone else's port 35995

you will likely notice 2 types of lines
one which is your host/ip:35995 connected to some random public/host ip with a random port

and another which is your host/ip<random port> connected with another random public/host ip with port 35995
  
  if it's your port 35995, it means that the other node reached out to you
if its their port 35995, it means your node reached out to them
  
  if the line says ESTABLISHED, it means it is an active connection
  
  you can run the netstat -a command without the grep and see it dumping info live
if you pipe it to the grep command
it will probably take a minute before you see anything
  
  
