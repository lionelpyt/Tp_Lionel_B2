# TP4 : Vers une maîtrise des OS Linux




🌞 **Faites une install manuelle de Rocky Linux**
```
[lionel@localhost ~]$ lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda           8:0    0   40G  0 disk
├─sda1        8:1    0   21G  0 part
│ ├─rl-root 253:0    0   10G  0 lvm  /
│ ├─rl-swap 253:1    0    1G  0 lvm  [SWAP]
│ ├─rl-home 253:2    0    5G  0 lvm  /home
│ └─rl-var  253:3    0    5G  0 lvm  /var
└─sda2        8:2    0    2G  0 part /boot
sr0          11:0    1 1024M  0 rom
```

🌞 **Remplissez votre partition `/home`**

```
[lionel@localhost /]$ dd if=/dev/zero of=/home/lionel/bigfile bs=4M count=5000
dd: error writing '/home/lionel/bigfile': No space left on device
1171+0 records in
1170+0 records out
4911116288 bytes (4.9 GB, 4.6 GiB) copied, 20.9666 s, 234 MB/s
[lionel@localhost /]$ lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda           8:0    0   40G  0 disk
├─sda1        8:1    0   21G  0 part
│ ├─rl-root 253:0    0   10G  0 lvm  /
│ ├─rl-swap 253:1    0    1G  0 lvm  [SWAP]
│ ├─rl-home 253:2    0    5G  0 lvm  /home
│ └─rl-var  253:3    0    5G  0 lvm  /var
└─sda2        8:2    0    2G  0 part /boot
sr0          11:0    1 1024M  0 rom
```

🌞 **Constater que la partition est pleine**
```
[lionel@localhost /]$ df -h
Filesystem           Size  Used Avail Use% Mounted on
devtmpfs             4.0M     0  4.0M   0% /dev
tmpfs                888M     0  888M   0% /dev/shm
tmpfs                356M  5.0M  351M   2% /run
/dev/mapper/rl-root  9.8G  1.1G  8.3G  12% /
/dev/sda2            2.0G  185M  1.7G  11% /boot
/dev/mapper/rl-home  4.9G  4.6G     0 100% /home
/dev/mapper/rl-var   4.9G  114M  4.5G   3% /var
tmpfs                178M     0  178M   0% /run/user/1000
```

🌞 **Agrandir la partition**

```
[lionel@localhost /]$ df -h
Filesystem           Size  Used Avail Use% Mounted on
devtmpfs             4.0M     0  4.0M   0% /dev
tmpfs                888M     0  888M   0% /dev/shm
tmpfs                356M  5.0M  351M   2% /run
/dev/mapper/rl-root  9.8G  1.1G  8.3G  12% /
/dev/sda2            2.0G  185M  1.7G  11% /boot
/dev/mapper/rl-home   22G  4.6G   17G  23% /home
/dev/mapper/rl-var   4.9G  114M  4.5G   3% /var
tmpfs                178M     0  178M   0% /run/user/1000
```

🌞 **Remplissez votre partition `/home`**
```
[lionel@localhost /]$ df -h
Filesystem           Size  Used Avail Use% Mounted on
devtmpfs             4.0M     0  4.0M   0% /dev
tmpfs                888M     0  888M   0% /dev/shm
tmpfs                356M  5.0M  351M   2% /run
/dev/mapper/rl-root  9.8G  1.1G  8.3G  12% /
/dev/sda2            2.0G  185M  1.7G  11% /boot
/dev/mapper/rl-home   22G   20G  1.1G  95% /home
/dev/mapper/rl-var   4.9G  114M  4.5G   3% /var
tmpfs                178M     0  178M   0% /run/user/1000
```


➜ **Eteignez la VM et ajoutez lui un disque de 40G**

🌞 **Utiliser ce nouveau disque pour étendre la partition `/home` de 40G**
```
[lionel@localhost /]$ df -h
Filesystem           Size  Used Avail Use% Mounted on
devtmpfs             4.0M     0  4.0M   0% /dev
tmpfs                888M     0  888M   0% /dev/shm
tmpfs                356M  5.0M  351M   2% /run
/dev/mapper/rl-root  9.8G  1.1G  8.3G  12% /
/dev/sda2            974M  185M  722M  21% /boot
/dev/mapper/rl-home   45G   17G   26G  40% /home
/dev/mapper/rl-var   4.9G  114M  4.5G   3% /var
tmpfs                178M     0  178M   0% /run/user/1000
```

# II. Gestion de users

Je vous l'ai jamais demandé, alors c'est limite un interlude obligé que j'ai épargné à tout le monde, mais les admins, vous y échapperez pas.

On va faire un petit exercice tout nul de gestion d'utilisateurs.

> *Si t'es si fort, ça prend même pas 2-3 min, alors fais-le :D*

🌞 **Gestion basique de users**
```
[lionel@localhost ~]$ cat /etc/group | grep -E "alice|bob|charlie|eve|backup"
alice:x:1002:
bob:x:1003:
charlie:x:1004:
eve:x:1005:
backup:x:1006:
admins:x:1007:alice,bob,charlie
```

🌞 **La conf `sudo` doit être la suivante**

| Qui est concerné | Quels droits                                                      | Doit fournir son password |
| ---------------- | ----------------------------------------------------------------- | ------------------------- |
| Groupe admins    | Tous les droits                                                   | Non                       |
| User eve         | Peut utiliser la commande `ls` en tant que l'utilisateur `backup` | Oui                       |

🌞 **Le dossier `/var/backup`**

- créez-le
- choisir des permissions les plus restrictives possibles (comme toujours, la base quoi) sachant que :
  - l'utilisateur `backup` doit pouvoir évoluer normalement dedans
  - les autres n'ont aucun droit
  - choisir donc des permissions les plus restrictives possibles

🌞 **Mots de passe des users, prouvez que**

- ils sont hashés en SHA512 (c'est violent)
- ils sont salés (c'est pas une blague si vous connaissez pas le terme, on dit "salted" en anglais aussi)

🌞 **User eve**

- elle ne peut que saisir `sudo ls` et rien d'autres avec `sudo`
- vous pouvez faire `sudo -l` pour voir vos droits `sudo` actuels

# III. Gestion du temps

![Timing](./img/timing.jpg)

Il y a un service qui tourne en permanence (ou pas) sur les OS modernes pour maintenir l'heure de la machine synchronisée avec l'heure que met à disposition des serveurs.

Le protocole qui sert à faire ça s'appelle NTP (Network Time Protocol, tout simplement). Il existe donc des serveurs NTP. Et le service qui tourne en permanence sur nos PCs/serveurs, c'est donc un client NTP.

Il existe des serveurs NTP publics, hébergés gracieusement, comme le projet [NTP Pool](https://www.ntppool.org).

🌞 **Je vous laisse gérer le bail vous-mêmes**

- déterminez quel service sur Rocky Linux est le client NTP par défaut
  - demandez à google, ou explorez la liste des services avec `systemctl list-units -t service -a`, ou les deux
- demandez à ce service de se synchroniser sur [les serveurs français du NTP Pool Project](https://www.ntppool.org/en/zone/fr)
- assurez-vous que vous êtes synchronisés sur l'heure de Paris

> systemd fournit un outil en ligne de commande `timedatectl` qui permet de voir des infos liées à la gestion du temps

# IV. Gestion de service

Dans cette section vous allez écrire :

- un ptit script de sauvegarde basique `backup.sh`
- un fichier de service `backup.service`
  - pour que notre OS lance le script de backup comme un service
- un fichier de timer `backup.timer`
  - pour que notre OS lance à intervalles réguliers le service de backup

> Je vous ai remis [le ptit cours scripting de l'an passé aussi](../../../cours/script/README.md).

➜ **Le script `backup.sh` :**

- est stocké dans le dossier `/srv/` de votre machine-test
- utilise `rsync` pour déplacer les fichiers/dossiers
- sauvegarde le contenu de `/etc/ssh/` et de `/var/log/`
- stocke les sauvegardes au format `.tar.gz`
- les sauvegardes sont stockées dans le dossier `/var/backup`
- ne stocke que 5 sauvegardes, les 5 plus récentes : à la 6ème, il supprime la plus ancienne avant d'effectuer la nouvelle sauvegarde
- les sauvegardes sont nommées `backup_yymmdd_hhmmss.tar.gz`
- doit afficher un message de succès quand la backup réussit (avec `echo`)

➜ **Le service `backup.service` :**

- est stocké dans `/etc/systemd/system/` pour que notre système le trouve
- lance le script `/srv/backup.sh`
- exécution en tant que l'utilisateur `backup`

> *Pour rappel dès qu'on modifie quoi que ce soit au contenu du dossier `/etc/systemd/system/`, il est nécessaire d'indiquer au système qu'il doit relire tous les fichiers avec `sudo systemctl daemon-reload`.*

➜ **Le timer `backup.timer` :**

- est stocké dans `/etc/systemd/system/` pour que notre système le trouve
- doit être exactement nommé comme le service, avec l'extension `.timer` à la place
- lance le service à intervalles réguliers : une fois tous les jours à 05h00
  - vous ferez le test que votre timer fonctionne bien avec d'autres intervalles
  - vous pouvez voir la date de la prochaine exécution (notamment) avec `systemctl list-timers --all`

🌞 **Rendu attendu**

- les trois fichiers dans le dépôt git
- un `systemctl start backup` suivi d'un `systemctl status backup` pour voir qu'il a bien été exécuté
- un `date` suivi d'un `systemctl list-timers --all` pour voir la date de la prochaine exécution
