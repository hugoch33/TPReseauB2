# TP3 INFRA : Premiers pas GNS, Cisco et VLAN
# 0. Prérequis
# I. Dumb switch

## 1. Topologie 1

🌞 Commençons simple

- sur PC1
```
 ip 10.2.2.2
```

- sur PC2
```
 ip 10.2.2.3
```

```
PC1> ping 10.2.2.3

84 bytes from 10.2.2.3 icmp_seq=1 ttl=64 time=1.389 ms
84 bytes from 10.2.2.3 icmp_seq=2 ttl=64 time=0.714 ms
84 bytes from 10.2.2.3 icmp_seq=3 ttl=64 time=0.546 ms
84 bytes from 10.2.2.3 icmp_seq=4 ttl=64 time=0.830 ms
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


🌞 Adressage

## 3. Setup topologie 2
