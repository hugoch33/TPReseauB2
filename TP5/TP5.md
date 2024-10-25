# TP5 INFRA : Intégration


# I. Tester

🌞 Récupérer l'application dans la VM hosting.tp5.b1

🌞 Essayer de lancer l'app
```bash
[hugo@hosting calculatrice]$ sudo ss -tulnp
Netid   State    Recv-Q   Send-Q      Local Address:Port        Peer Address:Port   Process
udp     UNCONN   0        0               127.0.0.1:323              0.0.0.0:*       users:(("chronyd",pid=661,fd=5))
udp     UNCONN   0        0                   [::1]:323                 [::]:*       users:(("chronyd",pid=661,fd=6))
tcp     LISTEN   0        1                 0.0.0.0:13337            0.0.0.0:*       users:(("python",pid=1253,fd=3))
tcp     LISTEN   0        128               0.0.0.0:22               0.0.0.0:*       users:(("sshd",pid=697,fd=3))
tcp     LISTEN   0        128                  [::]:22                  [::]:*       users:(("sshd",pid=697,fd=4))
```
🌞 Tester l'app depuis hosting.tp5.b1
```bash
[hugo@hosting calculatrice]$ python client.py
Calcul à envoyer: 25+2
27
```

# II. Intégrer

🌞 Créer un dossier /opt/calculatrice

🌞 Créer le fichier /etc/systemd/system/calculatrice.service

🌞 Démarrer le service

```bash
[hugo@hosting system]$ sudo ss -atnl | grep 13337
LISTEN 0      1            0.0.0.0:13337      0.0.0.0:*
[hugo@hosting system]$ sudo systemctl status calculatrice
● calculatrice.service - Super calculatrice réseau
     Loaded: loaded (/etc/systemd/system/calculatrice.service; disabled; preset: disabled)
     Active: active (running) since Fri 2024-10-25 14:57:11 CEST; 11min ago
   Main PID: 1519 (python)
      Tasks: 1 (limit: 11100)
     Memory: 3.3M
        CPU: 13ms
     CGroup: /system.slice/calculatrice.service
             └─1519 /usr/bin/python /opt/calculatrice/server.py

Oct 25 14:57:11 hosting systemd[1]: Started Super calculatrice réseau.
```
```bash
[hugo@hosting system]$ sudo ss -atnl | grep 13337
LISTEN 0      1            0.0.0.0:13337      0.0.0.0:*
```
```bash
[hugo@hosting calculatrice]$ python client.py
Calcul à envoyer: 25+4
29
```

🌞 Configurer une politique de redémarrage dans le fichier calculatrice.service
```bash
[hugo@hosting system]$ sudo cat calculatrice.service
[Unit]
Description=Super calculatrice réseau

[Service]
ExecStart=/usr/bin/python /opt/calculatrice/server.py
Restart=always
RestartSec=30
[Install]
WantedBy=multi-user.target
[hugo@hosting system]$
```

🌞 Tester que la politique de redémarrage fonctionne
```
[hugo@hosting /]$ sudo kill -9 1607
```

🌞 Ouverture automatique du firewall dans le fichier calculatrice.service

```bash
[hugo@hosting ~]$ sudo cat /etc/systemd/system/calculatrice.service
[Unit]
Description=Super calculatrice réseau

[Service]
ExecStart=/usr/bin/python /opt/calculatrice/server.py
Restart=always
RestartSec=3
ExecStartPre=/usr/bin/firewall-cmd --add-port=13337/tcp --permanent
ExecStartPre=/usr/bin/firewall-cmd --reload
ExecStopPost=/usr/bin/firewall-cmd --remove-port=13337/tcp --permanent
ExecStopPost=/usr/bin/firewall-cmd --reload
[Install]
WantedBy=multi-user.target
```

🌞 Vérifier l'ouverture automatique du firewall


```bash
PS C:\Users\chama> python .\Downloads\client.py
Calcul à envoyer: 2*5
10
```

# 3. Monitoring


```bash
[hugo@hosting netdata]$ sudo cat ./edit-config go.d/portcheck.conf | tail -23

jobs:
  - name: local
    host: 127.0.0.1
    ports:
      - 13337
```

🌞 Alerting Discord


# III. Héberger


