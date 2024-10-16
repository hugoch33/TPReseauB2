# TP3 INFRA : Premiers pas GNS, Cisco et VLAN
# 0. Prérequis
# I. Dumb switch

## 1. Topologie 1

🌞 Commençons simple

- sur PC1
```
 ip 10.3.1.1
```

- sur PC2
```
 ip 10.3.1.2
```

```
PC2> ping 10.3.1.1

84 bytes from 10.3.1.1 icmp_seq=1 ttl=64 time=1.233 ms
84 bytes from 10.3.1.1 icmp_seq=2 ttl=64 time=1.098 ms
84 bytes from 10.3.1.1 icmp_seq=3 ttl=64 time=1.238 ms
```


- afficher la CAM table du switch et vérifier les MAC qui s'y trouvent (Google pour ça, c'est pas dans le mémo)



```
IOU1#show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0050.7966.6800    DYNAMIC     Et0/0
   1    0050.7966.6801    DYNAMIC     Et0/1
Total Mac Addresses for this criterion: 2
```


# II. VLAN


## 3. Setup topologie 2

🌞 Adressage

```
PC3> ip 10.3.1.3
Checking for duplicate address...
PC3 : 10.3.1.3 255.255.255.0
```

```
PC3> ping 10.3.1.1

84 bytes from 10.3.1.1 icmp_seq=1 ttl=64 time=0.874 ms
84 bytes from 10.3.1.1 icmp_seq=2 ttl=64 time=1.020 ms
84 bytes from 10.3.1.1 icmp_seq=3 ttl=64 time=0.891 ms
84 bytes from 10.3.1.1 icmp_seq=4 ttl=64 time=0.973 ms
```
```
PC3> ping 10.3.1.2

84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=0.442 ms
84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=0.891 ms
84 bytes from 10.3.1.2 icmp_seq=3 ttl=64 time=0.592 ms
84 bytes from 10.3.1.2 icmp_seq=4 ttl=64 time=0.603 ms
```


🌞 Configuration des VLANs


```
IOU1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
IOU1(config)#vlan 10
IOU1(config-vlan)#name students
IOU1(config-vlan)#exit
IOU1(config)#vlan 20
IOU1(config-vlan)#name guests
IOU1(config-vlan)#exit
IOU1(config)#
```

```
IOU1#show vlan br

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Et0/3, Et1/0, Et1/1, Et1/2
                                                Et1/3, Et2/0, Et2/1, Et2/2
                                                Et2/3, Et3/0, Et3/1, Et3/2
                                                Et3/3
10   students                         active    Et0/0, Et0/1
20   guests                           active    Et0/2
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup
```
🌞 Vérif

```
PC1> ping 10.3.1.2

84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=0.399 ms
84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=0.552 ms
84 bytes from 10.3.1.2 icmp_seq=3 ttl=64 time=0.496 ms
```
```
PC1> ping 10.3.1.3

host (10.3.1.3) not reachable
```

# III. Ptite VM DHCP


🌞 VM dhcp.tp3.b2
```
[hugo@dhcp ~]$ sudo cat /etc/dhcp/dhcpd.conf
#
# DHCP Server Configuration file.
#   see /usr/share/doc/dhcp-server/dhcpd.conf.example
#   see dhcpd.conf(5) man page
#
# create new

# specify domain name

option domain-name     "srv.world";
# specify DNS server's hostname or IP address

option domain-name-servers     dlp.srv.world;
# default lease time

default-lease-time 600;
# max lease time

max-lease-time 7200;
# this DHCP server to be declared valid

authoritative;
# specify network address and subnetmask

subnet 10.3.1.0 netmask 255.255.255.0 {
    # specify the range of lease IP address
    range dynamic-bootp 10.3.1.100 10.3.1.200;
    # specify broadcast address
    option broadcast-address 10.3.1.255;
    # specify gateway
}
```
- vérifier avec le pc4 que vous pouvez récupérer une IP en DHCP
```
PC4> dhcp -r
DDORA
PC4> show IP 10.3.1.100/24
No router answered ICMPv6 Router Solicitation
```

```
# authoring-byte-order entry is generated, DO NOT DELETE
authoring-byte-order little-endian;

server-duid "\000\001\000\001.\232o>\010\000'\2240\202";

lease 10.3.1.100 {
  starts 4 2024/10/10 11:01:28;
  ends 4 2024/10/10 11:11:28;
  cltt 4 2024/10/10 11:01:28;
  binding state active;
  next binding state free;
  rewind binding state free;
  hardware ethernet 00:50:79:66:68:03;
  uid "\001\000Pyfh\003";
  client-hostname "PC4";
}
```
- vérifier avec le pc5 que vous ne pouvez PAS récupérer une IP en DHCP

```
PC5> dhcp -r
DDD
Can't find dhcp server
```