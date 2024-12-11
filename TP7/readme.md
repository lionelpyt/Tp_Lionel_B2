# TP7 INFRA : 3-tier architecture et redondance

### B. Tableau d'adressage

| Machine - Réseau  | `10.7.10.0/24` | `10.7.20.0/24` | `10.7.30.0/24` |
| ----------------- | -------------- | -------------- | -------------- |
| `r1.tp7.b1`       | `10.7.10.252`  | `10.7.20.252`  | `10.7.30.252`  |
| `r2.tp7.b1`       | `10.7.10.253`  | `10.7.20.253`  | `10.7.30.253`  |
| IP virtuelle HSRP | `10.7.10.254`  | `10.7.20.254`  | `10.7.30.254`  |
| `pc4.tp7.b1`      | `10.7.10.11`   | ❌             | ❌             |
| `pc1.tp7.b1`      | ❌             | `10.7.20.11`   | ❌             |
| `pc2.tp7.b1`      | ❌             | `10.7.20.12`   | ❌             |
| `pc5.tp7.b1`      | ❌             | ❌             | `10.7.30.11`   |

### C. Tableau des VLANs

- Association VLAN <> réseau IP

| VLAN              | VLAN 10 `clients` | VLAN 20 `admins` | VLAN 30 `servers` |
| ----------------- | ----------------- | ---------------- | ----------------- |
| Réseau IP associé | `10.7.10.0/24`    | `10.7.20.0/24`   | `10.7.30.0/24`    |

---

- Quel client est dans quel VLAN

| Machine - VLAN | VLAN 10 `clients` | VLAN 20 `admins` | VLAN 30 `servers` |
| -------------- | ----------------- | ---------------- | ----------------- |
| `pc4.tp7.b1`   | ✅                | ❌               | ❌                |
| `pc1.tp7.b1`   | ❌                | ✅               | ❌                |
| `pc2.tp7.b1`   | ❌                | ✅               | ❌                |
| `pc5.tp7.b1`   | ❌                | ❌               | ✅                |



➜ **VLANs everywhere**

- sur les switches :
  - trunk entre tous les équipements réseau
    - autorisez tous les VLANs à circuler partout
  - access vers les clients
    - référez-vous au tableau au dessus pour savoir quel client est dans quel VLAN
- sur les routeurs :
  - sous-interface pour permettre le routage inter-VLAN

### D. NAT

➜ **Config NAT sur les deux routeurs**

- ils doivent permettre de joindre l'extérieur

### E. Preuve et rendu

🌞 **`show-run`** 

- [Guide utilisateur](docs/guide.pdf)
- [Présentation du projet](https://example.com/slides)
- [Exemples de configuration](configs/exemple.yaml)


🌞 **depuis `pc4.tp7.b1`**

- `ping 10.7.30.11`
- `ping ynov.com`

## 3. Bonus

### A. ACL

🌞 **Le réseau `10.7.30.0/24`...**

- doit être injoignable depuis les autres réseaux
- SAUF si on essaie de contacter `10.7.30.67`
- genre si on ping cette IP c'est ok (depuis l'un des deux autres réseaux)
- mais si on ping n'importe quelle autre IP de ce réseau, ça fonctionne pas
- créez un VPCS qui porte l'IP `10.7.30.67` pour vos tests

### B. Spanning-tree

🌞 **Configuration de...**

- BPDUGuard
  - protection de trames STP non-désirées
  - genre un hacker se fait passer pour un switch, un voisin STP
- PortFast
  - permet aux ports STP de s'activer plus rapidement
- sur tous les switches

### C. Observe then destroy then observe

🌞 **Vérifier, à l'aide de commandes dédiées**

- l'état de l'agrégation LACP entre `sw1` et `sw2`
- l'état de la liaison HSRP entre `r1` et `r2`
- l'état de STP, par VLAN sur trois switches (un core, un distrib, un access)

🌞 **Couper le routeur prioritaire**

- éteins-le, débranche les câbles, peu importe
- un truc cool c'est le faire PENDANT qu'un client `ping` l'extérieur
  - pour voir, en temps réel, la bascule de l'IP

🌞 **Couper un switch crucial dans la topo STP**

- choisissez bien un switch qui a une place cruciale
  - genre un qui n'a pas tous ses ports en `BLK`
- shut it down !
- observe sur les autres switches la mise à jour de la topologie STP
- tu peux aussi lancer Wireshark sur quelques liens pour voir les trames STP échangées

![This is fine](./img/fine.jpg)

### D. DHCP Helper

🌞 **Setup un serveur DHCP**

- il a une IP dans `10.7.30.0/24`
- il attribue des IPs aux clients des 3 réseaux
- il est physiquement dans la même salle que `pc5.tp7.b1`
- configuration IP Helper sur les routeurs pour relayer les requêtes dans les 2 autres réseaux (dans lesquels le serveur DHCP ne se trouve pas lui même)

