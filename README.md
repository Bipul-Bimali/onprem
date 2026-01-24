# Setting up a Converter

## What we are looking for 
> We are trying to seperate our home environment with  a  lab environment while keeping the internet connectivity
>

 **Primary Router** -->  **converter** -->  **secondary/lab router**  -->> **lab environment**


_We can do it the other way around If you want  such that you can Access lab from home network but not the other way around_
___
### Collect necessary information 
> 
Home network IP : 10.0.0.1/24
converter IP :eth0 10.0.0.222 :eth1  10.8.0.9/30 (will keep 2 segments Lab A and Lab B )
Lab IP : 192.168.1.1/16
>

**Note**: _the IP set should be kept different to avoid network conflicts_
___
>
### List interfaces

```
ip a

```
>
eth0  and eth1
>
___
### Allow traffic to  pass
set net.ipv4.ip_forward = 1 
```
 sudo nano /etc/sysctl.conf
 sudo sysctl -p
```
### IP for downlink gateway
___
```
sudo ip addr add 10.10.0.1/30 dev eth0
sudo ip link set eth0 up 
```
___
### Configure NAT
> 
Configure and install iptables
>
```
 sudo apt install iptable
 sudo iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
 sudo iptables -A FORWARD -i eth0 -o wlan0 -j ACCEPT
 sudo iptables -A FORWARD -i wlan0 -o eth0 -m state --state RELATED,ESTABLISHED -j ACCEPT
```

### Configure Secondary Router
Configure secondary router such as
>
Router IP any ip withing the 10.10.0.1/30 subnet range
gateway will be the  ip of the eth0 we just set up 
>
___
___
## PROS 

*  An isolated lab environment without a seperate connection
___
## CONS 

* lab environment will have some latency  but can not abe accessed by anyone within the home environment
___

**NOTE**: *you will have to manually configure secondary router as we have not set DHCP in the converter 
you will giv the router an IP from the subnet  eth0 and gateway will be the ip on eth0  from the converter*
