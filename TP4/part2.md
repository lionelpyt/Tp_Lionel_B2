# II. NAT

On va ajouter une fonctionnalité au routeur : le NAT.

On va le connecter à internet (simulation du fait d'avoir une IP publique) et il va faire du NAT pour permettre à toutes les machines du réseau d'avoir un accès internet.

![Yellow cable](../img/yellow-cable.png)

## 1. Topologie 2

![Topologie 2](../img/topo2.png)

## 2. Adressage topologie 2

Les réseaux et leurs VLANs associés :

| Réseau    | Adresse        | VLAN associé |
| --------- | -------------- | ------------ |
| `clients` | `10.1.10.0/24` | 10           |
| `admins`  | `10.1.20.0/24` | 20           |
| `servers` | `10.1.30.0/24` | 30           |

L'adresse des machines au sein de ces réseaux :

| Node               | `clients`       | `admins`         | `servers`        |
| ------------------ | --------------- | ---------------- | ---------------- |
| `pc1.clients.tp4`  | `10.1.1.1/24`   | x                | x                |
| `pc2.clients.tp4`  | `10.1.1.2/24`   | x                | x                |
| `adm1.admins.tp4`  | x               | `10.1.20.1/24`   | x                |
| `web1.servers.tp4` | x               | x                | `10.1.30.1/24`   |
| `r1`               | `10.1.1.254/24` | `10.1.20.254/24` | `10.1.30.254/24` |

## 3. Setup topologie 2

🌞 **Ajoutez le noeud Cloud à la topo**
```
R1#show ip int br FastEthernet1/0
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet1/0            10.0.3.16       YES DHCP   up                    up  

PC1> ping 1.1.1.1
1.1.1.1 icmp_seq=1 timeout
84 bytes from 1.1.1.1 icmp_seq=2 ttl=53 time=28.890 ms
84 bytes from 1.1.1.1 icmp_seq=3 ttl=53 time=27.179 ms
```

🌞 **Configurez le NAT**

🌞 **Test**

```
PC1> ping google.com
google.com resolved to 142.251.37.46

84 bytes from 142.251.37.46 icmp_seq=1 ttl=112 time=30.956 ms
84 bytes from 142.251.37.46 icmp_seq=2 ttl=112 time=25.335 ms
```
