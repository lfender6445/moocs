DHCP (Dynamic Host Configuration Protocol) is a protocol used to provide quick, automatic, and central management for the distribution of IP addresses within a network.

DHCP is also used to configure the proper subnet mask, default gateway, and DNS server information on the device.

A DHCP server is used to issue unique IP addresses and automatically configure other network information. In most homes and small businesses, the router acts as the DHCP server. In large networks, a single computer might act as the DHCP server.

each container connected to a private virtual network bridge

each virtual network routes through nat firewall on host

all containers on a virtual network can talk to each other without -p

best practice is to create a new virtuanetwork for each app

```sh
# -p to expose port 80 of our nginx
# host port forwards traffic into port of container
docker container run -p 80:80 --name webhost -d nginx
# get ip
docker container inspect --format '{{.NetworkSettings.IPAddress}}' webhost
# above md returns something like 172.123.423.23 so its not same network
# any traffic passing through your interface is natted
# docker adds new network, bridge or docker 0
# and that network is connected to your interface so it can get out
# you can create multiple bridge networks
# for example, you could expose apache on the host by doing -p 8080:80
# which could in turn talk to another container, mysql
# but youd only need to expose apache because they exist on same bridge
```

## network management
```sh
# show all created networks
docker network ls
# inspet bridge network, containers, their ips
docket network inspect bridge
# docker subnet is usuallay 172.17.0.0/16
docker network ls
# ^ host network gains performance by skipping virtual network but sacrifices secruity
# ^ network none equivalent of interface not attached to anything
# to create a network:
docker network create my_app_net
# --network can also be used to create network at the same time as a container
# connect a network to 2 networks
docker network connect <id_of_network> <id_of_container>
# by default, you only expose what you want to expose to outside host
# all external ports closed by default
```

by default, bridge network does ship with dns resolution by default (you can technically use --link) with dns resolution so its usually easier to create your own network and add containers to them

eg
```sh
# assuming new_nginx container is already in my_app_net
docker container run -d --name my_nginx --network my_app_net nginx`
# 2 containers now in the network
docker container exec -it my_nginx ping new_nginx
# dns resolution just works!
# vips are unreliable, as you start and stop containers they could
# shift ips based on the order you start them
```

containers should rely on ips for inter-communication
dns for friendly names is built in
always prefer custom networks

```
# install centos, run bash, remove on exit
docker container run --rm -it centos:7 bash
```
dns round robin is a poor mans load balancer, used to serve up multiple hosts behind a single ip. its not perfect but works for rotation

you can use --net-alias to obtain simiar effect with docker containers

## what is an image
- an image is app binaries and dependencies
- metadata about the image and how to run it

Not a complete os - no kernel, no kernel modules (drivers)

as small as one file, if you like
or you could have a very big image like ubuntu
