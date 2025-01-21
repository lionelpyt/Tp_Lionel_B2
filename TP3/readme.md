# TP3 INFRA : Premiers pas GNS, Cisco et VLAN


## 2. Adressage topologie 1

| Node  | IP            |
| ----- | ------------- |
| `pc1` | `10.3.1.1/24` |
| `pc2` | `10.3.1.2/24` |

## 3. Setup topologie 1

🌞 **Commençons simple**
```
PC1> ip 10.3.1.1

PC1> ping 10.3.1.2

84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=0.238 ms
84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=1.478 ms
84 bytes from 10.3.1.2 icmp_seq=3 ttl=64 time=0.660 ms
84 bytes from 10.3.1.2 icmp_seq=4 ttl=64 time=2.545 ms

IOU7#show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0050.7966.6800    DYNAMIC     Et0/0
   1    0050.7966.6801    DYNAMIC     Et0/1


```
# II. VLAN

**Le but dans cette partie va être de tester un peu les *VLANs*.**

On va rajouter **un troisième client** qui, bien que dans le même réseau, sera **isolé des autres grâce aux *VLANs***.

**Les *VLANs* sont une configuration à effectuer sur les *switches*.** C'est les *switches* qui effectuent le blocage.

Le principe est simple :

- déclaration du VLAN sur tous les switches
  - un VLAN a forcément un ID (un entier)
  - bonne pratique, on lui met un nom
- sur chaque switch, on définit le VLAN associé à chaque port
  - genre "sur le port 35, c'est un client du VLAN 20 qui est branché"

![VLAN FOR EVERYONE](./img/get_a_vlan.jpg)

## 1. Topologie 2

![Topologie 2](./img/topo2.png)

## 2. Adressage topologie 2

| Node  | IP            | VLAN |
| ----- | ------------- | ---- |
| `pc1` | `10.3.1.1/24` | 10   |
| `pc2` | `10.3.1.2/24` | 10   |
| `pc3` | `10.3.1.3/24` | 20   |

### 3. Setup topologie 2

🌞 **Adressage**
```
PC3> ip 10.3.1.3

PC1> ping 10.3.1.2
84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=1.603 ms
84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=2.906 ms

PC1> ping 10.3.1.3
84 bytes from 10.3.1.3 icmp_seq=1 ttl=64 time=3.940 ms
84 bytes from 10.3.1.3 icmp_seq=2 ttl=64 time=2.936 ms

PC2> ping 10.3.1.3
84 bytes from 10.3.1.3 icmp_seq=1 ttl=64 time=3.702 ms

```
🌞 **Configuration des VLANs**



🌞 **Vérif**
```
PC1> ping 10.3.1.2

84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=3.399 ms
84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=1.826 ms

PC3> ping 10.3.1.1

host (10.3.1.1) not reachable

PC3> ping 10.3.1.2

host (10.3.1.2) not reachable
```



# III. Ptite VM DHCP

On va ajouter une VM dans la topologie, histoire que vous voyiez cet aspect de GNS.

| Node          | IP              | VLAN |
| ------------- | --------------- | ---- |
| `pc1`         | `10.3.1.1/24`   | 10   |
| `pc2`         | `10.3.1.2/24`   | 10   |
| `pc3`         | `10.3.1.3/24`   | 20   |
| `pc4`         | X               | 20   |
| `pc5`         | X               | 10   |
| `dhcp.tp3.b2` | `10.3.1.253/24` | 20   |

🌞 **VM `dhcp.tp3.b2`**



