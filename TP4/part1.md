# I. Topo 1 : VLAN et Routing



## 1. Topologie 1


## 2. Adressage topologie 1

Les réseaux et leurs VLANs associés :

| Réseau    | Adresse        | VLAN associé |
| --------- | -------------- | ------------ |
| `clients` | `10.1.10.0/24`  | 10           |
| `admins`  | `10.1.20.0/24` | 20           |
| `servers` | `10.1.30.0/24` | 30           |

L'adresse des machines au sein de ces réseaux :

| Node               | `clients`       | `admins`         | `servers`        |
| ------------------ | --------------- | ---------------- | ---------------- |
| `pc1.clients.tp4`  | `10.1.10.1/24`   | x                | x                |
| `pc2.clients.tp4`  | `10.1.10.2/24`   | x                | x                |
| `adm1.admins.tp4`  | x               | `10.1.20.1/24`   | x                |
| `web1.servers.tp4` | x               | x                | `10.1.30.1/24`   |
| `r1`               | `10.1.10.254/24` | `10.1.20.254/24` | `10.1.30.254/24` |

## 3. Setup topologie 1

🖥️ VM `web1.servers.tp4`, déroulez la [Checklist VM Linux](#checklist-vm-linux) dessus

🌞 **Adressage**

```
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.1.10.1/24

PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.1.10.2/24

PC3> show ip

NAME        : PC3[1]
IP/MASK     : 10.1.20.1/24

[lionel@web1 ~]$ ip a | grep enp0s8
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    inet 10.1.30.1/24 brd 10.1.30.255 scope global noprefixroute enp0s8
```

🌞 **Configuration des VLANs**


```
IOU1#show vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Et1/1, Et1/2, Et1/3, Et2/0
                                                Et2/1, Et2/2, Et2/3, Et3/0
                                                Et3/1, Et3/2, Et3/3
10   VLAN0010                         active    Et0/0, Et0/1
20   VLAN0020                         active    Et0/2
30   VLAN0030                         active    Et0/3



IOU1#show interfaces trunk

Port        Mode             Encapsulation  Status        Native vlan
Et1/0       on               802.1q         trunking      1

Port        Vlans allowed on trunk
Et1/0       1-4094

Port        Vlans allowed and active in management domain
Et1/0       1,10,20,30

Port        Vlans in spanning tree forwarding state and not pruned
Et1/0       1,10,20,30


           
```

🌞 **Config du *routeur***
```
R1#show ip int brief
Interface                  IP-Address      OK? Method Status           Protocol
FastEthernet0/0            unassigned      YES unset  administratively down
down                                                                                                                               
FastEthernet0/0.10         10.1.10.254     YES manual administratively down down                                                                                                                               
FastEthernet0/0.20         10.1.20.254     YES manual administratively down down                                                                                                                               
FastEthernet0/0.30         10.1.30.254     YES manual administratively down 
down   
```

🌞 **Vérif**

```
Verif ping routeur:

PC1> ping 10.1.10.254
84 bytes from 10.1.10.254 icmp_seq=1 ttl=255 time=11.779 ms
84 bytes from 10.1.10.254 icmp_seq=2 ttl=255 time=10.902 ms

PC2> ping 10.1.10.254
84 bytes from 10.1.10.254 icmp_seq=1 ttl=255 time=9.191 ms
84 bytes from 10.1.10.254 icmp_seq=2 ttl=255 time=2.628 ms

ADMIN> ping 10.1.20.254
84 bytes from 10.1.20.254 icmp_seq=1 ttl=255 time=9.945 ms
84 bytes from 10.1.20.254 icmp_seq=2 ttl=255 time=9.170 ms

[lionel@web1 ~]$ ping 10.1.30.254
PING 10.1.30.254 (10.1.30.254) 56(84) bytes of data.
64 bytes from 10.1.30.254: icmp_seq=1 ttl=255 time=69.5 ms
64 bytes from 10.1.30.254: icmp_seq=2 ttl=255 time=1029 ms
64 bytes from 10.1.30.254: icmp_seq=3 ttl=255 time=46.5 ms

Verif ping VPC:

PC1> ping 10.1.20.1
10.1.20.1 icmp_seq=1 timeout
84 bytes from 10.1.20.1 icmp_seq=2 ttl=63 time=558.613 ms

PC1> ping 10.1.30.1
84 bytes from 10.1.30.1 icmp_seq=1 ttl=63 time=85.928 ms
84 bytes from 10.1.30.1 icmp_seq=2 ttl=63 time=756.391 ms

[lionel@web1 ~]$ ping 10.1.10.1
PING 10.1.10.1 (10.1.10.1) 56(84) bytes of data.
64 bytes from 10.1.10.1: icmp_seq=1 ttl=63 time=17.0 ms
64 bytes from 10.1.10.1: icmp_seq=2 ttl=63 time=12.8 ms

[lionel@web1 ~]$ ping 10.1.20.1
PING 10.1.20.1 (10.1.20.1) 56(84) bytes of data.
64 bytes from 10.1.20.1: icmp_seq=1 ttl=63 time=1051 ms
64 bytes from 10.1.20.1: icmp_seq=2 ttl=63 time=1003 ms

ADMIN> ping 10.1.10.2
10.1.10.2 icmp_seq=1 timeout
84 bytes from 10.1.10.2 icmp_seq=2 ttl=63 time=17.616 ms

ADMIN> ping 10.1.30.1
84 bytes from 10.1.30.1 icmp_seq=1 ttl=63 time=20.999 ms
84 bytes from 10.1.30.1 icmp_seq=2 ttl=63 time=13.876 ms

```

