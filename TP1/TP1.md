# TP1 : Maîtrise réseau du votre poste

## I. Basics

☀️ Carte réseau WiFi

- l'adresse MAC de votre carte WiFi

```
C:\Users\chama>ipconfig /all


Carte réseau sans fil Wi-Fi :

    ☀️Adresse physique . . . . . . . . . . . : 92-D5-3B-F5-79-F5☀️
    Adresse IPv4. . . . . . . . . . . . . .: 10.33.72.150(préféré)
    Masque de sous-réseau. . . . . . . . . : 255.255.240.0
```
- l'adresse IP de votre carte WiFi

```
C:\Users\chama>ipconfig /all

Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Killer(R) Wi-Fi 6 AX1650i 160MHz Wireless Network Adapter (201NGW)
   Adresse physique . . . . . . . . . . . : 92-D5-3B-F5-79-F5
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::9a24:18b8:e579:d17f%3(préféré)
   ☀️Adresse IPv4. . . . . . . . . . . . . .: 10.33.72.150(préféré)☀️
  
```
- le masque de sous-réseau du réseau LAN auquel vous êtes connectés en WiFi

```
C:\Users\chama>ipconfig /all

Carte réseau sans fil Wi-Fi :

   Adresse IPv6 de liaison locale. . . . .: fe80::9a24:18b8:e579:d17f%3(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.72.150(préféré)
  ☀️ Masque de sous-réseau. . . . . . . . . : 255.255.240.0☀️
   Bail obtenu. . . . . . . . . . . . . . : jeudi 3 octobre 2024 
```


☀️ Déso pas déso

- l'adresse de réseau du LAN auquel vous êtes connectés en WiFi
```
Adresse du réseau : 10.33.64.0
```
- l'adresse de broadcast

```
Adresse Broadcast : 10.33.79.255
```
- le nombre d'adresses IP disponibles dans ce réseau

```
4,094
```

☀️ Hostname

```
C:\Users\chama>hostname
LAPTOP-J4TG9IM0
```

☀️ Passerelle du réseau

- l'adresse IP de la passerelle du réseau

```
Passerelle par défaut. . . . . . . . . : 10.33.79.254
```

- l'adresse MAC de la passerelle du réseau

```
C:\Users\chama>arp -a

Interface : 10.33.72.150 --- 0x3
  Adresse Internet      Adresse physique      Type
  10.33.79.254          7c-5a-1c-d3-d8-76     dynamique
```

☀️ Serveur DHCP et DNS

- l'adresse IP du serveur DHCP qui vous a filé une IP

```
C:\Users\chama>ipconfig /all

Carte réseau sans fil Wi-Fi :
   
   Bail expirant. . . . . . . . . . . . . : vendredi 4 octobre 2024 14:06:21
   Passerelle par défaut. . . . . . . . . : 10.33.79.254
☀️Serveur DHCP . . . . . . . . . . . . . : 10.33.79.254☀️
   IAID DHCPv6 . . . . . . . . . . . : 59954491
   DUID de client DHCPv6. . . . . . . . : 00-03-00-01-92-D5-3B-F5-79-F5
   Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8
```

- l'adresse IP du serveur DNS que vous utilisez quand vous allez sur internet

```
C:\Users\chama>ipconfig /all

Carte réseau sans fil Wi-Fi :
   
   Bail expirant. . . . . . . . . . . . . : vendredi 4 octobre 2024 14:06:21
   Passerelle par défaut. . . . . . . . . : 10.33.79.254
   Serveur DHCP . . . . . . . . . . . . . : 10.33.79.254
   IAID DHCPv6 . . . . . . . . . . . : 59954491
   DUID de client DHCPv6. . . . . . . . : 00-03-00-01-92-D5-3B-F5-79-F5
   ☀️Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8☀️
```

☀️ Table de routage

- dans votre table de routage, laquelle est la route par défaut

```
IPv4 Table de routage
===========================================================================
Itinéraires actifs :
Destination réseau    Masque réseau  Adr. passerelle   Adr. interface Métrique
          ☀️0.0.0.0          0.0.0.0     10.33.79.254     10.33.72.150☀️     30
```

## II. Go further

☀️ Hosts ?

- faites en sorte que pour votre PC, le nom b2.hello.vous corresponde à l'IP 1.1.1.1

```
# Copyright (c) 1993-2009 Microsoft Corp.
#
# This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
#
# This file contains the mappings of IP addresses to host names. Each
# entry should be kept on an individual line. The IP address should
# be placed in the first column followed by the corresponding host name.
# The IP address and the host name should be separated by at least one
# space.
#
# Additionally, comments (such as these) may be inserted on individual
# lines or following the machine name denoted by a '#' symbol.
#
# For example:
#
#      102.54.94.97     rhino.acme.com          # source server
#       38.25.63.10     x.acme.com              # x client host

# localhost name resolution is handled within DNS itself.
#	127.0.0.1       localhost
#	::1             localhost
☀️1.1.1.1 b2.hello.vous☀️
```

```
C:\Users\chama>ping b2.hello.vous

Envoi d’une requête 'ping' sur b2.hello.vous [1.1.1.1] avec 32 octets de données :
Réponse de 1.1.1.1 : octets=32 temps=18 ms TTL=55
Réponse de 1.1.1.1 : octets=32 temps=15 ms TTL=55
Réponse de 1.1.1.1 : octets=32 temps=15 ms TTL=55
Réponse de 1.1.1.1 : octets=32 temps=15 ms TTL=55

Statistiques Ping pour 1.1.1.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 15ms, Maximum = 18ms, Moyenne = 15ms
```

☀️ Go mater une vidéo youtube et déterminer, pendant qu'elle tourne...

- l'adresse IP du serveur auquel vous êtes connectés pour regarder la vidéo


```
C:\Windows\System32>netstat -n -a -b

[svchost.exe]
  UDP    0.0.0.0:49656          8.8.4.4:443
```

- le port du serveur auquel vous êtes connectés

```
C:\Windows\System32>netstat -n -a -b

[svchost.exe]
  UDP    0.0.0.0:49656          8.8.4.4:☀️443☀️
```

- le port que votre PC a ouvert en local pour se connecter au port du serveur distant

```
C:\Windows\System32>netstat -n -a -b

[svchost.exe]
  UDP    0.0.0.0:☀️49656☀️        8.8.4.4:443
```
☀️ Requêtes DNS

- à quelle adresse IP correspond le nom de domaine www.thinkerview.com

```
C:\Users\chama>nslookup www.thinkerview.com
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    www.thinkerview.com
Addresses:  2a06:98c1:3120::7
          2a06:98c1:3121::7
          ☀️188.114.97.7☀️
          188.114.96.7
```

- à quel nom de domaine correspond l'IP 143.90.88.12

```
C:\Users\chama>nslookup 143.90.88.12
Serveur :   dns.google
Address:  8.8.8.8

Nom :    ☀️EAOcf-140p12.ppp15.odn.ne.jp☀️
Address:  143.90.88.12
```

☀️ Hop hop hop

- par combien de machines vos paquets passent quand vous essayez de joindre www.ynov.com


```
C:\Users\chama>tracert  www.ynov.com

Détermination de l’itinéraire vers www.ynov.com [172.67.74.226]
avec un maximum de 30 sauts :

  1     2 ms     1 ms     2 ms  10.33.79.254
  2     2 ms     2 ms     2 ms  145.117.7.195.rev.sfr.net [195.7.117.145]
  3     9 ms     2 ms     2 ms  237.195.79.86.rev.sfr.net [86.79.195.237]
  4     3 ms     3 ms     3 ms  196.224.65.86.rev.sfr.net [86.65.224.196]
  5    11 ms    11 ms    11 ms  164.147.6.194.rev.sfr.net [194.6.147.164]
  6    65 ms     *        *     162.158.20.24
  7    17 ms    16 ms    15 ms  162.158.20.240
  8    15 ms    15 ms    14 ms  172.67.74.226

Itinéraire déterminé.
```

☀️ IP publique

- l'adresse IP publique de la passerelle du réseau (le routeur d'YNOV donc si vous êtes dans les locaux d'YNOV quand vous faites le TP)



l'adresse IP publique de la passerelle du réseau : 195.117.146

## III. Le requin

- livrez moi des captures réseau avec uniquement ce que je demande et pas 40000 autres paquets autour



☀️ Capture DNS

[Wireshark DNS](./wireshark/CaptureDNS.pcap)

☀️ Capture TCP

[Wireshark TCP](./wireshark/.pcap)
