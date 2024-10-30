# II. Intégrer

## 1. Environnement

Petite section pour préparer un environnement correct. On va faire le strict minimum.

C'est une application installée à la main, on va la positionner un peu à l'arrache dans `/opt`.

🌞 **Créer un dossier /opt/calculatrice**

```
[lionel@hosting ~]$ ls opt/calculatrice/
client.py  server.py
```

## 2. systemd service

Dans cette section, on va créer un fichier `calculatrice.service` qui nous permettra de saisir des commandes comme :

```bash
$ sudo systemctl start calculatrice
$ sudo systemctl enable calculatrice

$ sudo journalctl -xe -u calculatrice
```

### B. Service basique

🌞 **Créer le fichier `/etc/systemd/system/calculatrice.service`**
```
[lionel@hosting ~]$ ls -al /etc/systemd/system/ | grep calculatrice
-rwxr-xr-x.  1 root root  152 Oct 24 16:03 calculatrice.service


[lionel@hosting ~]$ cat /etc/systemd/system/calculatrice.service
[Unit]
Description=Super calculatrice réseau

[Service]
ExecStart=/usr/bin/python /opt/calculatrice/server.py

[Install]
WantedBy=multi-user.target
```


🌞 **Démarrer le service**
```
[lionel@hosting ~]$ systemctl status calculatrice
● calculatrice.service - Super calculatrice réseau
     Loaded: loaded (/etc/systemd/system/calculatrice.service; disabled; preset: disabled)
     Active: active (running) since Thu 2024-10-24 16:23:55 CEST; 2s ago
   Main PID: 1984 (python)
      Tasks: 1 (limit: 4674)
     Memory: 3.3M
        CPU: 16ms
     CGroup: /system.slice/calculatrice.service
             └─1984 /usr/bin/python /opt/calculatrice/server.py

Oct 24 16:23:55 hosting.tp5.b1 systemd[1]: Started Super calculatrice réseau.
```

  - le service tourne derrière un port donné avec un `ss`
  - c'est fonctionnel : vous pouvez utiliser l'app en lançant le client

### C. Amélioration du service

➜ **Redémarrage automatique**

- si un jour le service est down, pour n'importe quelle raison, il serait bon qu'il redémarre automatiquement
- c'est une des features de systemd, on va configurer ça !

🌞 **Configurer une politique de redémarrage** dans le fichier `calculatrice.service`

```
[lionel@hosting ~]$ cat /etc/systemd/system/calculatrice.service
[Unit]
Description=Super calculatrice réseau

[Service]
ExecStart=/usr/bin/python /opt/calculatrice/server.py
Restart=on-failure
RestartSec=30
[Install]
WantedBy=multi-user.target
```

🌞 **Tester que la politique de redémarrage fonctionne**

```
[lionel@hosting ~]$ sudo kill -9 2250
[sudo] password for lionel:
[lionel@hosting ~]$ systemctl status calculatrice.service
● calculatrice.service - Super calculatrice réseau
     Loaded: loaded (/etc/systemd/system/calculatrice.service; enabled; preset: disabled)
     Active: activating (auto-restart) (Result: signal) since Thu 2024-10-24 17:25:08 CE>
    Process: 2250 ExecStart=/usr/bin/python /opt/calculatrice/server.py (code=killed, si>
   Main PID: 2250 (code=killed, signal=KILL)
        CPU: 15ms

Oct 24 17:25:08 hosting.tp5.b1 systemd[1]: calculatrice.service: Main process exited, co>
Oct 24 17:25:08 hosting.tp5.b1 systemd[1]: calculatrice.service: Failed with result 'sig>


[lionel@hosting ~]$ systemctl status calculatrice.service
● calculatrice.service - Super calculatrice réseau
     Loaded: loaded (/etc/systemd/system/calculatrice.service; enabled; preset: disabled)
     Active: active (running) since Thu 2024-10-24 17:25:38 CEST; 3s ago
   Main PID: 2275 (python)
      Tasks: 1 (limit: 4674)
     Memory: 3.3M
        CPU: 17ms
     CGroup: /system.slice/calculatrice.service
             └─2275 /usr/bin/python /opt/calculatrice/server.py
```
➜ **Firewall !**

- pour le moment vous avez fait tous les tests localement
  - donc on a pas eu besoin de toucher au firewall
  - le firewall de Rocky Linux ne bloque aucun traffic depuis/vers `127.0.0.1`
- quand on va héberger l'app sur le réseau de l'école et accueillir des clients potentiels, il faudra ouvrir un port firewall

🌞 **Ouverture automatique du firewall** dans le fichier `calculatrice.service`

```
[lionel@hosting ~]$ sudo cat /etc/systemd/system/calculatrice.service |grep firewall
ExecStartPre=/usr/bin/firewall-cmd --add-port=13337/tcp --permanent
ExecStartPre=/usr/bin/firewall-cmd --reload
ExecStopPost=/usr/bin/firewall-cmd --remove-port=13337/tcp --permanent
ExecStopPost=/usr/bin/firewall-cmd --reload
```

🌞 **Vérifier l'ouverture automatique du firewall**
```
[lionel@hosting ~]$ systemctl status calculatrice.service
● calculatrice.service - Super calculatrice réseau
     Loaded: loaded (/etc/systemd/system/calculatrice.service; enabled; preset: disabled)
     Active: active (running) since Fri 2024-10-25 13:55:49 CEST; 10s ago
    Process: 1383 ExecStartPre=/usr/bin/firewall-cmd --add-port=13337/tcp --permanent (code=exited, status=0/SUCCESS)
    Process: 1384 ExecStartPre=/usr/bin/firewall-cmd --reload (code=exited, status=0/SUCCESS)
   Main PID: 1394 (python)
      Tasks: 1 (limit: 4674)
     Memory: 3.3M
        CPU: 159ms
     CGroup: /system.slice/calculatrice.service
             └─1394 /usr/bin/python /opt/calculatrice/server.py


[lionel@hosting ~]$ sudo firewall-cmd --list-all | grep ports
ports: 13337/tcp


PS C:\Users\lione> python C:\Users\lione\client.py
Calcul à envoyer: 1/1
1.0


[lionel@hosting ~]$ systemctl status calculatrice.service
○ calculatrice.service - Super calculatrice réseau
     Loaded: loaded (/etc/systemd/system/calculatrice.service; enabled; preset: disabled)
     Active: inactive (dead) since Fri 2024-10-25 13:59:29 CEST; 1min 11s ago
   Duration: 3min 39.402s
    Process: 1383 ExecStartPre=/usr/bin/firewall-cmd --add-port=13337/tcp --permanent (code=exited, status=0/SUCCESS)
    Process: 1384 ExecStartPre=/usr/bin/firewall-cmd --reload (code=exited, status=0/SUCCESS)
    Process: 1394 ExecStart=/usr/bin/python /opt/calculatrice/server.py (code=exited, status=0/SUCCESS)
    Process: 1406 ExecStopPost=/usr/bin/firewall-cmd --remove-port=13337/tcp --permanent (code=exited, status=0/SUCCESS)
    Process: 1407 ExecStopPost=/usr/bin/firewall-cmd --reload (code=exited, status=0/SUCCESS)
   Main PID: 1394 (code=exited, status=0/SUCCESS)
        CPU: 314ms

[lionel@hosting ~]$ sudo firewall-cmd --list-all | grep ports
  ports:

```

## 3. Monitoring

🌞 **Installer Netdata** sur `hosting.tp5.b1`
```
[lionel@hosting ~]$ [ "2d9d5910c018f9c3b5d12f77cdc95c75" = "$(curl -Ss https://get.netdata.cloud/kickstart.sh | md5sum | cut -d ' ' -f 1)" ] && echo "OK, VALID" || echo "FAILED, INVALID"
OK, VALID
```

🌞 **Configurer une sonde TCP**

- c'est à dire qu'on va demander à Netdata de faire une requête TCP vers un port
  - si le port répond, Netdata considère que le service est up
  - sinon, il considère que c'est down
- ça va nous permettre de suivre un peu en temps réel si notre service est accessible depuis le réseau
- [cette section de la doc](https://learn.netdata.cloud/docs/data-collection/synthetic-checks/tcp-endpoints) parle de comment faire, lisez et check les exemples

> Dans le monde réel, le serveur de monitoring qui fait ce genre de checks est souvent installé sur une autre machine. Comme ça on simule vraiment un accès par le réseau à l'application, pour savoir si elle est disponible.

🌞 **Alerting Discord**

- vous connaissez la chanson : j'aimerai que vous récupériez des alertes automatiquement sur Discord
- [cette section de la doc qui en parle](https://learn.netdata.cloud/docs/alerting/notifications/agent-dispatched-notifications/discord)
- testez que vous recevez une alerte quand vous coupez le service, et que votre sonde TCP n'a plus de réponse

![Monitoring](./img/monitoring.jpg)

