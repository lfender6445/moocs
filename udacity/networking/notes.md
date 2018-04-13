# Reserved IP addresses

https://classroom.udacity.com/courses/ud256/lessons/7118148621/concepts/72314704150923

An IPv4 address is actually a 32-bit numeric value. We usually write it as a dotted quad — four decimal numbers, separated by dots, like 206.190.36.45 or 180.149.132.47. Each of the four numbers written down like this represents one octet or 8-bit value.

But writing an IP address as four decimal numbers is just a convention to make it easier to read. When it’s used between computers, an address is transmitted in binary, just like all the other data sent over the network.

However, not all of the possible 32-bit values are used for real addresses. Some of them are used for special applications that use addresses differently. Some of them are reserved for internal private networks. Some of them are for testing or documentation.

In fact, just over one-eighth of all possible IPv4 addresses are set aside for something other than addressing public hosts. But as you'll see, even if we did use all 32-bit addresses to represent public hosts, that still wouldn’t really be enough.

## how many ipv4 addresses are there?

1 bit = 2 addresses or 2^1
2 bit = 4 addresses or 2^2
3 bit = 8 addresses or 2^3

2^32 - 1 = 4,294,967,295

There are 7 billion humans on the planet

## networks

all of the addresses on a specific network block, share a particular prefix
they all start the same but differ after particular bit position

computers that are on the same network block can communicate with each other without going through a router. not all networks are the same size.

size can be inferred by the length of the prefix

shorter prefix == larger network
longer prefix == smaller network

## subnets
A subnetwork or subnet is a logical subdivision of an IP network. The practice of dividing a network into two or more networks is called subnetting. Computers that belong to a subnet are addressed with a common, identical, most-significant bit-group in their IP address

## 24 bit prefix
8 bit, 16, 24, 32 (dotted quad)

given a network with these addresses

- 198.51.100.142
- 198.51.100.17

we can see there are 8 bits left for host part of the addresses (because 198.51.100 is the prefix, all thats left is the last octet)

2^8 is 256 so about 256 slots are available on the network

But remember, top and bottom of network blocks are reservered,
and the very first is usually the router.

For this reason its a good idea to subtract 3

The network above  could be written as

  198.51.100.0 / 24 Network Block
  2^8 possible addresses (256 - 3), so about 253 possible addresses

## subnets
The subnet mask for a /24 netblock in dotted quad is 255.255.255.0

Some systems use hex instead of decimal (ff,ff,ff,00)

## whats subnet mask for /16 network block?

255.255.0.0

prefixes do not have to be octets.

net blocks can be split into subnets to make management easier and control traffic

example: 22 bits of prefix part, 10 bits of host part is a /22 network which doesnt evenly
divide by 8.

another example,
171.64.0.0/14 Stanford network block

14 is less than 2 octets of network part (14 is less than `16)

So while all addresses on this network will have the same 8 bits (represented by 171), only 6 bits of the second octet will be fixed (giving us 2 extra bits to play with for addresses).

first 8 fixed, (14 - 8 === 6)

and bits in the second octet will be free, meaning for possible values (2^2), giving used

171.64, 171.65, 171.66, and 171.67 in the netblock

# questions
So what is the nubnet mask for /14?

- The subnet mask for /14 netblock will have 14 bits for the network part (all 1's)
- that leaves 18 bits for the host part (all 0s)

11111111 | 11111100 | 00000000 | 00000000

  translates to

255.252.0.0

How many addressess? 2^(32-14) == 262144

## packets

packets countain origin and destination info (ips)


## hosts

hosts dont have ip addressess - interfaces on hosts have ip addresses
- wired ethernet eth0
- wireless interface wlan0
- loopback interface lo
- tunnels
- virtualbox

# interfaces
- `ip addr show` will show interfaces
- `ifconfig | less` on mac

## gateways

your router acts as a gateway to the rest of the internet.

- `ip route show default` on linux
- `netstat -nr` linux mac unix

Your gateway is directly on the same local network as your machine itself, so the ping time is generally very fast. If it's not, or if there's packet loss, that would usually indicate a network hardware problem.

# NAT

gateways provides one public address for all devices on the same network, giving private ip address
to all connected devices

Private ip addresses come off of one of 3 resevered net blocks (aka rfc1918 addressesses)
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16

most common on home routers: 192.168.0.0/24
with 192.168.0.1 as a typical default gateway.

These are outlined in an internet standard called RFC1918

Private IP addresses are used with a system called NAT (Network Address Translation)

NAT is a workaround and not a solution. The router maintains a map of traffic from internal / private ips are connected to external public facing ips (eg visiting a website).

This makes it difficult to do things like run a server at home, because your computer doesn't know its real public address

# private and public
private addresses arent secret or personal, but because they're only accessible through the local
network. they cant be used directly because they arent unique, but all are behind different NAT routers.

the public address to your network is exposed through your router or mobile provider, or the likes.

websites see your public address

`ipconfig getifaddr en1` on mac for wifi

# ipv6
128 bits (16 octets)

The world doesnt need nat with ipv6 because theres enough addresses to be unique.

usage is about 10% according to google at end of 2015

notes: my mobile network supports ipv6 but home network does not

# protocol stack
layers

application, transport, internet, hardware

```
- http or ssh (resources, urls, verbs, cookies) in flask, apache, browers
-- tcp (ports, sessions, stream sockets) in os kernel sys libs
--- ip (address packets) in os kernel routers
---- hardware (wifi, access points, passswords) in device drivers
```

not all network applications use tcp (ping uses ICMP)
dns uses UDP as does network time protocol (NTP) to keep your time and date in sync

tcp connections are also known as sessions

`sudo tcpdump -n port 53` - dump all dns requests
