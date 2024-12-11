# TP6 INFRA : STP, OSPF, bigger infra

![Routing issues](./img/routing.png)

## Sommaire

- [TP6 INFRA : STP, OSPF, bigger infra](#tp6-infra--stp-ospf-bigger-infra)
  - [Sommaire](#sommaire)
  - [0. Setup](#0-setup)
  - [I. STP](#i-stp)
    - [1. Basic setup](#1-basic-setup)
    - [2. Fonctionnalités avancées](#2-fonctionnalités-avancées)
  - [II. OSPF](#ii-ospf)
  - [III. DHCP relay](#iii-dhcp-relay)
  - [IV. Bonus](#iv-bonus)
    - [1. ACL](#1-acl)


🌞 **Configurer STP sur les 3 switches**

 ```
 IOU3#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.0100
             Cost        100
             Port        2 (Ethernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.0300
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Altn BLK 100       128.1    P2p
Et0/1               Root FWD 100       128.2    P2p
Et0/2               Desg FWD 100       128.3    P2p
Et0/3               Desg FWD 100       128.4    P2p
Et1/0               Desg FWD 100       128.5    P2p
Et1/1               Desg FWD 100       128.6    P2p
Et1/2               Desg FWD 100       128.7    P2p
Et1/3               Desg FWD 100       128.8    P2p
Et2/0               Desg FWD 100       128.9    P2p
Et2/1               Desg FWD 100       128.10   P2p
Et2/2               Desg FWD 100       128.11   P2p
Et2/3               Desg FWD 100       128.12   P2p
Et3/0               Desg FWD 100       128.13   P2p
Et3/1               Desg FWD 100       128.14   P2p
Et3/2               Desg FWD 100       128.15   P2p
Et3/3               Desg FWD 100       128.16   P2p

```

🌞 **Altérer le spanning-tree** en désactivant un port

status : LIS puis LRN

```
IOU3#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.0100
             Cost        100
             Port        2 (Ethernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.0300
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- -----------------------------
Et0/0               Desg FWD 100       128.1    P2p
Et0/1               Root FWD 100       128.2    P2p
Et0/2               Desg FWD 100       128.3    P2p
Et0/3               Desg FWD 100       128.4    P2p
Et1/0               Desg FWD 100       128.5    P2p
Et1/1               Desg FWD 100       128.6    P2p
Et1/2               Desg FWD 100       128.7    P2p
Et1/3               Desg FWD 100       128.8    P2p
Et2/0               Desg FWD 100       128.9    P2p
Et2/1               Desg FWD 100       128.10   P2p
Et2/2               Desg FWD 100       128.11   P2p
Et2/3               Desg FWD 100       128.12   P2p
Et3/0               Desg FWD 100       128.13   P2p
Et3/1               Desg FWD 100       128.14   P2p
Et3/2               Desg FWD 100       128.15   P2p
Et3/3               Desg FWD 100       128.16   P2p
```


🌞 **Altérer le spanning-tree** en modifiant le coût d'un lien
```
IOU3#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.0100
             Cost        100
             Port        2 (Ethernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.0300
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Altn BLK 100       128.1    P2p
Et0/1               Root LRN 100       128.2    P2p
Et0/2               Desg LRN 100       128.3    P2p
Et0/3               Desg LRN 100       128.4    P2p
Et1/0               Desg LRN 100       128.5    P2p
Et1/1               Desg LRN 100       128.6    P2p
Et1/2               Desg LRN 100       128.7    P2p
Et1/3               Desg LRN 100       128.8    P2p
Et2/0               Desg LRN 100       128.9    P2p
Et2/1               Desg LRN 100       128.10   P2p
Et2/2               Desg LRN 100       128.11   P2p
Et2/3               Desg LRN 100       128.12   P2p
Et3/0               Desg LRN 100       128.13   P2p
Et3/1               Desg LRN 100       128.14   P2p
Et3/2               Desg LRN 100       128.15   P2p
Et3/3               Desg LRN 100       128.16   P2p


IOU3#
IOU3#
IOU3#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
IOU3(config)#interface e0/1
IOU3(config-if)#spanning-tree vlan 1 cost 700
IOU3(config-if)#
IOU3#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.0100
             Cost        200
             Port        1 (Ethernet0/0)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.0300
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Root LIS 100       128.1    P2p
Et0/1               Altn BLK 700       128.2    P2p
Et0/2               Desg FWD 100       128.3    P2p
Et0/3               Desg FWD 100       128.4    P2p
Et1/0               Desg FWD 100       128.5    P2p
Et1/1               Desg FWD 100       128.6    P2p
Et1/2               Desg FWD 100       128.7    P2p
```
### 2. Fonctionnalités avancées

Bonus si vous voulez aller un peu plus loin et connaître des fonctionnalités un peu plus avancées :

- regardez du côté de *port fast*
- et aussi *bpduguard*

## II. OSPF

**Passons à OSPF donc** : pour mettre en place un routage dynamique.

On va se cantonner à le setup de façon simple, et ensuite on mettra en place un service qui consomme ce routage en partie III.

![Topo OSPF](./img/topo_ospf.png)

> Ce sont les *areas* OSPF qui sont représentées en couleur, pas des réseaux. 🌸

➜ Tableau d'adressage

- la logique de l'adressage que je vous propose :
  - choix de masque
    - du `/24` pour les réseaux où y'a des clients
      - classique, simple
    - du `/30` pour les réseaux entre les routeurs
      - comme ça, on permet vraiment explicitement que deux IPs sur ces réseaux
  - choix des octets
    - `10.6.` pour les deux premiers octets
      - 10 pas chiant comme d'hab
      - 6 pour TP6 comme d'hab
    - pour le troisième octet
      - entre les routeurs : `10.6.13.` l'octet qui suit :
        - 13 indique le réseau entre le routeur 1 et le routeur 3
        - 13 et pas 31 parce que je lis de gauche à droite perso
      - réseaux clients : `10.6.1.`
        - arbitraire, y'a un réseau 1, un réseau 2, etc.

| Node          | `10.6.1.0/24` | `10.6.2.0/24` | `10.6.3.0/24` | `10.6.41.0/30` | `10.6.13.0/30` | `10.6.21.0/30` | `10.6.23.0/30` | `10.6.52.0/30` |
| ------------- | ------------- | ------------- | ------------- | -------------- | -------------- | -------------- | -------------- | -------------- |
| `waf.tp6.b1`  | `10.6.1.11`   | ❌            | ❌            | ❌             | ❌             | ❌             | ❌             | ❌             |
| `dhcp.tp6.b1` | `10.6.1.253`  | ❌            | ❌            | ❌             | ❌             | ❌             | ❌             | ❌             |
| `meo.tp6.b1`  | ❌            | `10.6.2.11`   | ❌            | ❌             | ❌             | ❌             | ❌             | ❌             |
| `john.tp6.b1` | ❌            | ❌            | `10.6.3.11`   | ❌             | ❌             | ❌             | ❌             | ❌             |
| `R1`          | ❌            | ❌            | `10.6.3.254`  | `10.6.41.1`    | `10.6.13.1`    | `10.6.21.1`    | ❌             | ❌             |
| `R2`          | ❌            | ❌            | ❌            | ❌             | ❌             | `10.6.21.2`    | `10.6.23.2`    | `10.6.52.2`    |
| `R3`          | ❌            | ❌            | ❌            | ❌             | `10.6.13.2`    | ❌             | `10.6.23.1`    | ❌             |
| `R4`          | `10.6.1.254`   | `10.6.2.254`   | ❌            | `10.6.41.2`    | ❌            | ❌             | ❌             | ❌             |
| `R5`          | ❌            | ❌            | ❌            | ❌            | ❌             | ❌             | ❌             | `10.6.52.1`    |

🌞 **Montez la topologie**

- IP statiques sur tout le monde
  - assurez-vous que les pings passent au sein de chacun des LANs
  - au fur et à mesure que vous configurez
- configuration d'un NAT sur le routeur connecté à internet
- **aucune route statique ne doit être ajoutée nulle part**
- définissez aux clients (VPCS ou VMs) des IPs statiques et définissez leur gateway
  - ils auront toujours pas internet, car leur routeur n'a pas internet !
- aucune configuration particulière à faire sur `dhcp.tp6.b2` pour le moment, on fera ça en partie III.
  [-](-.md) juste une IP statique, pas de setup particulier

➜ Bref...

- IP statiques partout
- clients avec une gateway définie
- et un NAT sur le routeur de bordure

On va pas ajouter toutes les routes sur tous les routeurs, ce serait une giga plaie à faire, à maintenir, et peu résilient.

OSPF donc.

🌞 **Configurer OSPF sur tous les routeurs**

- tous les routeurs doivent partager tous les réseaux auxquels ils sont connectés
- un petit `show running-config` où vous enlevez ce que vous n'avez pas tapé pour le rendu !
- et un `show ip ospf neighbor` + `show ip route` sur chaque routeur
- n'oubliez pas de partager la route par défaut de R5 avec une commande OSPF spécifique [voir mémo](../../../cours/memo/cisco.md)

> Référez-vous [au mémo Cisco](../../../cours/memo/cisco.md) pour les commandes OSPF.

🌞 **Test**

- faites des `ping` dans tous les sens
- c'est simple hein : normalement tout le monde peut ping tout le monde
- et même tout le monde a internet y compris les clients
- mettez moi quelques `ping` dans le compte-rendu

🦈 **`tp6_ospf.pcapng`**

- capturez des trames OSPF là où vous voulez
- interprétez les trames que vous capturez

## III. DHCP relay

➜ **Un problème très récurrent pour pas dire omniprésent avec DHCP c'est que ça marche que dans un LAN.**

Si t'as un serveur DHCP, et plein de réseaux comme c'est le cas ici, c'est le bordel :

- un DHCP Request, qui part en broadcast ne passe pas un routeur
  - il est cantonné au LAN
- en effet, pour changer de réseau, il faut construire des paquets IP
  - or, un DHCP request, c'est juste une trame Ethernet, pas de paquet IP dedans
- et donc, quand tu fais ton DHCP Request c'est ça que tu cherches : avoir une IP
- dans notre topo actuelle, impossible que John contacte le serveur DHCP

➜ **DHCP Relay !**

- on va demander à un routeur, s'il reçoit des trames DHCP de les faire passer vers notre serveur DHCP
- si le serveur DHCP le supporte, il répondra donc au routeur, qui fera passer au client

> *Spoiler alert, le serveur qu'on utilise depuis l'an dernier, fourni dans les dépôts Rocky, le supporte.*

🌞 **Configurer un serveur DHCP** sur `dhcp.tp6.b1`

- même setup que d'habitude [(c'est ce lien que tu cherches ?)](https://www.server-world.info/en/note?os=Rocky_Linux_8&p=dhcp&f=1)
- votre serveur DHCP donne des IPs dans les réseaux
  - `10.6.1.0/24`
    - de `10.6.1.100` à `10.6.1.200`
    - informe les clients de l'adresse de la passerelle de ce réseau
    - informe les clients de l'adresse d'un serveur DNS : `1.1.1.1`
  - `10.6.3.0/24`
    - de `10.6.3.100` à `10.6.3.200`
    - informe les clients de l'adresse de la passerelle de ce réseau
    - informe les clients de l'adresse d'un serveur DNS : `1.1.1.1`
- pour le compte-rendu ça me suffit :
  - `sudo cat /etc/dhcp/dhcpd.conf`
  - `systemctl status dhcpd`

🌞 **Configurer un DHCP relay sur la passerelle de John**

- vérifier que Waf et John peuvent récupérer une IP en DHCP
- check les logs du serveur DHCP pour voir les DORA
  - je veux ces 4 lignes de logs dans le compte-rendu
  - pour John et pour Waf
- la conf sur le routeur qui est la passerelle de John c'est :

```cisco
R1#conf t
R1(config)#interface fastEthernet 2/0 # interface qui va recevoir des requêtes DHCP
R1(config-if)#ip helper-address <DHCP_SERVER_IP_ADDRESS>
```

> *Ui c'est tout. Bah... quoi de plus ? Il a juste besoin de savoir à qui faire passer les requêtes !*

## IV. Bonus

### 1. ACL

C'est un peu moche que les clients puissent `ping` les IPs des routeurs de l'autre côté de l'infra.

Normalement, il peut joindre sa passerelle, internet, épuicétou.

🌞 **Configurer une access-list**

- ça se fait sur les routeurs
- le but :
  - les clients peuvent ping leur passerelle
  - et internet
  - épuicétou

![it ain't one](./img/it_aint_one.jpg)

