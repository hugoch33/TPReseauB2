# TP4 INFRA : Router-on-a-stick

# I. Topo 1 : VLAN et Routing


🌞 Adressage

- définissez les IPs statiques sur toutes les machines sauf le routeur
```
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.1.10.1/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20011
RHOST:PORT  : 127.0.0.1:20012
MTU         : 1500
```

```
PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.1.10.2/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20013
RHOST:PORT  : 127.0.0.1:20014
MTU         : 1500
```

```
VPCS> show ip

NAME        : VPCS[1]
IP/MASK     : 10.1.20.1/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:02
LPORT       : 20015
RHOST:PORT  : 127.0.0.1:20016
MTU         : 1500
```
```
[hugo@web ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:38:ae:ea brd ff:ff:ff:ff:ff:ff
    inet 10.1.30.1/24 brd 10.1.30.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe38:aeea/64 scope link
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:90:7d:f0 brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.11/24 brd 10.1.1.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe90:7df0/64 scope link
       valid_lft forever preferred_lft forever
```
🌞 Configuration des VLANs


- déclaration des VLANs sur le switch sw1
- ajout des ports du switches dans le bon VLAN (voir le tableau d'adressage de la topo 2 juste au dessus)
```
IOU1#show vlan br

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Et0/3, Et1/1, Et1/2, Et1/3
                                                Et2/0, Et2/1, Et2/2, Et2/3
                                                Et3/0, Et3/1, Et3/2, Et3/3
10   clients                          active    Et0/0, Et0/1
20   admins                           active    Et0/2
30   servers                          active    Et1/0
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup
```

🌞 Config du routeur

- attribuez ses IPs au routeur

```
R1#show ip interface brief
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            unassigned      YES NVRAM  administratively down down
FastEthernet0/0.10         10.1.10.254     YES NVRAM  administratively down down
FastEthernet0/0.20         10.1.20.254     YES manual administratively down down
FastEthernet0/0.30         10.1.30.254     YES manual administratively down down
FastEthernet1/0            unassigned      YES NVRAM  administratively down down
FastEthernet1/1            unassigned      YES NVRAM  administratively down down
FastEthernet2/0            unassigned      YES NVRAM  administratively down down
FastEthernet2/1            unassigned      YES NVRAM  administratively down down
```

🌞 Vérif

- tout le monde doit pouvoir ping le routeur sur l'IP qui est dans son réseau

```
PC1> ping 10.1.10.254

10.1.10.254 icmp_seq=1 timeout
84 bytes from 10.1.10.254 icmp_seq=2 ttl=255 time=10.657 ms
84 bytes from 10.1.10.254 icmp_seq=3 ttl=255 time=2.033 ms
```
```
PC2> ping 10.1.10.254

10.1.10.254 icmp_seq=1 timeout
84 bytes from 10.1.10.254 icmp_seq=2 ttl=255 time=11.178 ms
84 bytes from 10.1.10.254 icmp_seq=3 ttl=255 time=1.140 ms
84 bytes from 10.1.10.254 icmp_seq=4 ttl=255 time=7.887 ms
84 bytes from 10.1.10.254 icmp_seq=5 ttl=255 time=7.748 ms
```
```
VPCS> ping 10.1.20.254

10.1.20.254 icmp_seq=1 timeout
84 bytes from 10.1.20.254 icmp_seq=2 ttl=255 time=8.695 ms
84 bytes from 10.1.20.254 icmp_seq=3 ttl=255 time=10.656 ms
84 bytes from 10.1.20.254 icmp_seq=4 ttl=255 time=4.821 ms
```
```
[hugo@web ~]$ ping 10.1.30.254
PING 10.1.30.254 (10.1.30.254) 56(84) bytes of data.
64 bytes from 10.1.30.254: icmp_seq=1 ttl=255 time=21.7 ms
64 bytes from 10.1.30.254: icmp_seq=2 ttl=255 time=5.58 ms
64 bytes from 10.1.30.254: icmp_seq=3 ttl=255 time=10.1 ms
```
- ajoutez une route par défaut sur les VPCS

```
PC1> ip 10.1.10.1/24 10.1.10.254
```
```
PC2> ip 10.1.10.2/24 10.1.10.254
```

```
VPCS> ip 10.1.20.1/24 10.1.20.254
Checking for duplicate address...
VPCS : 10.1.20.1 255.255.255.0 gateway 10.1.20.254
```

- ajoutez une route par défaut sur la machine virtuelle

```
[hugo@web ~]$ sudo nano /etc/sysconfig/network-scripts/route-enp0s3
[hugo@web ~]$ ip route show
default via 10.1.1.254 dev enp0s3 proto static metric 100
10.1.1.0/24 dev enp0s8 proto kernel scope link src 10.1.1.11 metric 101
10.1.1.254 dev enp0s3 proto static scope link metric 100
10.1.2.0/24 via 10.1.1.254 dev enp0s3 proto static metric 100
10.1.30.0/24 dev enp0s3 proto kernel scope link src 10.1.30.1 metric 100
10.1.30.1 via 10.1.30.254 dev enp0s3
[hugo@web ~]$ sudo systemctl restart NetworkManager
```



- testez des ping entre les réseaux

```
PC2> ping 10.1.30.1

84 bytes from 10.1.30.1 icmp_seq=1 ttl=63 time=18.832 ms
84 bytes from 10.1.30.1 icmp_seq=2 ttl=63 time=15.103 ms
```

```
VPCS> ping 10.1.10.1

84 bytes from 10.1.10.1 icmp_seq=1 ttl=63 time=30.836 ms
84 bytes from 10.1.10.1 icmp_seq=2 ttl=63 time=13.441 ms
```

```
[hugo@web ~]$ ping 10.1.10.1
PING 10.1.10.1 (10.1.10.1) 56(84) bytes of data.
64 bytes from 10.1.10.1: icmp_seq=1 ttl=63 time=20.2 ms
64 bytes from 10.1.10.1: icmp_seq=2 ttl=63 time=22.3 ms
```


# II. NAT


## 3. Setup topologie 2
 
🌞 Ajoutez le noeud Cloud à la topo


- côté routeur, il faudra récupérer un IP en DHCP (voir le mémo Cisco)

```
R1#sh ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            unassigned      YES NVRAM  up                    up
FastEthernet0/0.10         10.1.10.254     YES NVRAM  up                    up
FastEthernet0/0.20         10.1.20.254     YES NVRAM  up                    up
FastEthernet0/0.30         10.1.30.254     YES NVRAM  up                    up
FastEthernet1/0            10.0.3.16       YES DHCP   up                    up
```

- vous devriez pouvoir ping 1.1.1.1

```
R1#ping 1.1.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 1.1.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 12/21/32 ms
```


🌞 Configurez le NAT
```
R1(config)#interface fastEthernet1/0
R1(config-if)#ip nat outside
R1(config-if)#exit
R1(config)#interface fastEthernet0/0
R1(config-if)#ip nat inside
R1(config-if)#exit
```
- configurez l'utilisation d'un DNS

sur les VPCS
```
 ip dns 1.1.1.1
```
sur la VM :
```
[hugo@web ~]$ sudo cat /etc/resolv.conf
[sudo] password for hugo:
# Generated by NetworkManager
nameserver 8.8.8.8
nameserver 1.1.1.1
```
🌞 Test

```
PC1> ping ynov.com
ynov.com resolved to 172.67.74.226

84 bytes from 172.67.74.226 icmp_seq=1 ttl=53 time=30.040 ms
84 bytes from 172.67.74.226 icmp_seq=2 ttl=53 time=24.034 ms
```
```
PC2> ping ynov.com
ynov.com resolved to 104.26.11.233

84 bytes from 104.26.11.233 icmp_seq=1 ttl=53 time=29.561 ms
84 bytes from 104.26.11.233 icmp_seq=2 ttl=53 time=25.351 ms
```
```
VPCS> ping ynov.com
ynov.com resolved to 172.67.74.226

84 bytes from 172.67.74.226 icmp_seq=1 ttl=53 time=31.130 ms
84 bytes from 172.67.74.226 icmp_seq=2 ttl=53 time=21.963 ms
84 bytes from 172.67.74.226 icmp_seq=3 ttl=53 time=22.516 ms
```
```
[hugo@web ~]$ ping ynov.com
PING ynov.com (172.67.74.226) 56(84) bytes of data.
64 bytes from 172.67.74.226 (172.67.74.226): icmp_seq=1 ttl=53 time=30.4 ms
64 bytes from 172.67.74.226 (172.67.74.226): icmp_seq=2 ttl=53 time=22.9 ms
```

# III. Add a building

## 2. Adressage topologie 3

