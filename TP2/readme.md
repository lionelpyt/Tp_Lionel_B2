
☀️ Sur **`node1.lan1.tp2`**
```
- afficher ses cartes réseau

[lionel@node1lan1 network-scripts]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:79:4e:cc brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.11/24 brd 10.1.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe79:4ecc/64 scope link
       valid_lft forever preferred_lft forever

- afficher sa table de routage

[lionel@node1lan1 ~]$ ip r s
10.1.1.0/24 dev enp0s3 proto kernel scope link src 10.1.1.11 metric 100
10.1.2.0/24 via 10.1.1.254 dev enp0s3 proto static metric 100

- prouvez qu'il peut joindre `node2.lan2.tp2`

[lionel@node1lan1 ~]$ ping 10.1.2.12
PING 10.1.2.12 (10.1.2.12) 56(84) bytes of data.
64 bytes from 10.1.2.12: icmp_seq=1 ttl=63 time=1.86 ms
64 bytes from 10.1.2.12: icmp_seq=2 ttl=63 time=1.43 ms
64 bytes from 10.1.2.12: icmp_seq=3 ttl=63 time=1.79 ms

- prouvez avec un `traceroute` que le paquet passe bien par `router.tp2`

[lionel@node1lan1 ~]$ traceroute 10.1.2.12
traceroute to 10.1.2.12 (10.1.2.12), 30 hops max, 60 byte packets
 1  10.1.1.254 (10.1.1.254)  1.469 ms  1.052 ms  0.998 ms
 2  10.1.2.12 (10.1.2.12)  1.800 ms !X  3.001 ms !X  2.436 ms !X
```
# II. Interlude accès internet

![No internet](./img/no%20internet.jpg)

**On va donner accès internet à tout le monde.** Le routeur aura un accès internet, et permettra à tout le monde d'y accéder : il sera la passerelle par défaut des membres du LAN1 et des membres du LAN2.

**Ajoutez une carte NAT au routeur pour qu'il ait un accès internet.**

☀️ **Sur `router.tp2`**
```
- prouvez que vous avez un accès internet (ping d'une IP publique)
[lionel@router ~]$ ping 1.1.1.1
PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=54 time=19.8 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=54 time=19.8 ms
64 bytes from 1.1.1.1: icmp_seq=3 ttl=54 time=20.4 ms

- prouvez que vous pouvez résoudre des noms publics (ping d'un nom de domaine public)

[lionel@router ~]$ ping google.com
PING google.com (216.58.214.78) 56(84) bytes of data.
64 bytes from fra15s10-in-f78.1e100.net (216.58.214.78): icmp_seq=1 ttl=116 time=20.4 ms
64 bytes from fra15s10-in-f78.1e100.net (216.58.214.78): icmp_seq=2 ttl=116 time=20.3 ms
```
☀️ **Accès internet LAN1 et LAN2**
```
[lionel@node2lan1 ~]$ sudo cat /etc/sysconfig/network-scripts/route-enp0s3
10.1.2.1/24 via 10.1.1.254 dev enp0s3
default via 10.1.1.254 dev enp0s3

- prouvez que `node2.lan1.tp2` a un accès internet :

[lionel@node2lan1 ~]$ ping 1.1.1.1
PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=53 time=21.0 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=53 time=21.2 ms

[lionel@node2lan1 ~]$ ping google.com
PING google.com (216.58.214.78) 56(84) bytes of data.
64 bytes from fra15s10-in-f14.1e100.net (216.58.214.78): icmp_seq=1 ttl=115 time=21.7 ms
64 bytes from fra15s10-in-f78.1e100.net (216.58.214.78): icmp_seq=2 ttl=115 time=23.5 ms

```

# III. Services réseau


☀️ **Sur `web.lan2.tp2`**
```
[lionel@weblan2 ~]$ ss -tulpn | grep :80
tcp   LISTEN 0      511          0.0.0.0:80        0.0.0.0:*
tcp   LISTEN 0      511             [::]:80           [::]:*
```

☀️ **Sur `node1.lan1.tp2`**

```
[lionel@node1lan1 ~]$ sudo cat /etc/hosts | grep site
10.1.2.12 site_nul.tp2

[lionel@node1lan1 ~]$ curl site_nul.tp2
<h1>coucou<h1>
```

