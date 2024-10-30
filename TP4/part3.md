# III. Add a building

On a acheté un nouveau bâtiment, faut tirer et configurer un nouveau switch jusque là-bas.

On va en profiter pour setup un serveur DHCP pour les clients qui s'y trouvent.

## 1. Topologie 3

![Topo 3](../img/topo3.png)

## 2. Adressage topologie 3

Les réseaux et leurs VLANs associés :

| Réseau    | Adresse        | VLAN associé |
| --------- | -------------- | ------------ |
| `clients` | `10.1.10.0/24`  | 10           |
| `admins`  | `10.1.20.0/24` | 20           |
| `servers` | `10.1.30.0/24` | 30           |

L'adresse des machines au sein de ces réseaux :

| Node                | `clients`       | `admins`         | `servers`        |
| ------------------- | --------------- | ---------------- | ---------------- |
| `pc1.clients.tp4`   | `10.1.10.1/24`   | x                | x                |
| `pc2.clients.tp4`   | `10.1.10.2/24`   | x                | x                |
| `pc3.clients.tp4`   | DHCP            | x                | x                |
| `pc4.clients.tp4`   | DHCP            | x                | x                |
| `pc5.clients.tp4`   | DHCP            | x                | x                |
| `dhcp1.clients.tp4` | `10.1.10.253/24` | x                | x                |
| `adm1.admins.tp4`   | x               | `10.1.20.1/24`   | x                |
| `web1.servers.tp4`  | x               | x                | `10.1.30.1/24`   |
| `r1`                | `10.1.10.254/24` | `10.1.20.254/24` | `10.1.30.254/24` |

## 3. Setup topologie 3

Vous pouvez partir de la topologie 2.

🌞  **Vous devez me rendre le `show running-config` de tous les équipements**

```
R1#show running-config
Building configuration...

Current configuration : 1482 bytes
!
version 12.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R1
!
boot-start-marker
boot-end-marker
!
!
no aaa new-model
no ip icmp rate-limit unreachable
!
!
ip cef
no ip domain lookup
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
ip tcp synwait-time 5
!
!
!
!
!
interface FastEthernet0/0
 no ip address
 ip nat inside
 ip virtual-reassembly
 duplex full
!
interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 10.1.10.254 255.255.255.0
!
interface FastEthernet0/0.20
 encapsulation dot1Q 20
 ip address 10.1.20.254 255.255.255.0
!
interface FastEthernet0/0.30
 encapsulation dot1Q 30
 ip address 10.1.30.254 255.255.255.0
!
interface FastEthernet1/0
 ip address dhcp
 duplex auto
 speed auto
!
interface FastEthernet1/1
 no ip address
 ip nat outside
 ip virtual-reassembly
 shutdown
 duplex auto
 speed auto
!
interface FastEthernet2/0
 no ip address
 shutdown
 duplex auto
 speed auto
!
interface FastEthernet2/1
 no ip address
 shutdown
 duplex auto
 speed auto
!
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
ip nat inside source list 1 interface FastEthernet1/1 overload
!
access-list 1 permit any
no cdp log mismatch duplex
!
!
!
control-plane
!
!
!
!
!
!
gatekeeper
 shutdown
!
!
line con 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
 stopbits 1
line aux 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
 stopbits 1
line vty 0 4
 login
!
!
end
```
```
IOU1#show running-config
Building configuration...

Current configuration : 1691 bytes
!
! Last configuration change at 12:09:19 UTC Fri Oct 18 2024
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname IOU1
!
boot-start-marker
boot-end-marker
!
!
logging discriminator EXCESS severity drops 6 msg-body drops EXCESSCOLL
logging buffered 50000
logging console discriminator EXCESS
!
no aaa new-model
!
!
!
!
!
no ip icmp rate-limit unreachable
!
!
!
no ip domain-lookup
no ip cef
no ipv6 cef
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
interface Ethernet0/0
 switchport access vlan 10
 switchport mode access
!
interface Ethernet0/1
 switchport access vlan 10
 switchport mode access
!
interface Ethernet0/2
 switchport access vlan 20
 switchport mode access
!
interface Ethernet0/3
 switchport access vlan 30
 switchport mode access
!
interface Ethernet1/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet1/1
!
interface Ethernet1/2
!
interface Ethernet1/3
!
interface Ethernet2/0
!
interface Ethernet2/1
!
interface Ethernet2/2
!
interface Ethernet2/3
!
interface Ethernet3/0
!
interface Ethernet3/1
!
interface Ethernet3/2
!
interface Ethernet3/3
!
interface Vlan1
 no ip address
 shutdown
!
ip forward-protocol nd
!
ip tcp synwait-time 5
ip http server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
!
!
!
!
control-plane
!
!
line con 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
line aux 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
line vty 0 4
 login
!
!
!
end
```
```
IOU2#show running-config
Building configuration...

Current configuration : 1609 bytes
!
! Last configuration change at 14:09:41 UTC Fri Oct 18 2024
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname IOU2
!
boot-start-marker
boot-end-marker
!
!
logging discriminator EXCESS severity drops 6 msg-body drops EXCESSCOLL
logging buffered 50000
logging console discriminator EXCESS
!
no aaa new-model
!
!
!
!
!
no ip icmp rate-limit unreachable
!
!
!
no ip domain-lookup
no ip cef
no ipv6 cef
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/2
!
interface Ethernet0/3
!
interface Ethernet1/0
!
interface Ethernet1/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet1/2
!
interface Ethernet1/3
!
interface Ethernet2/0
!
interface Ethernet2/1
!
interface Ethernet2/2
!
interface Ethernet2/3
!
interface Ethernet3/0
!
interface Ethernet3/1
!
interface Ethernet3/2
!
interface Ethernet3/3
!
interface Vlan1
 no ip address
 shutdown
!
ip forward-protocol nd
!
ip tcp synwait-time 5
ip http server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
!
!
!
!
control-plane
!
!
line con 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
line aux 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
line vty 0 4
 login
!
!
!
end
```
```
IOU3#show running-config
Building configuration...

Current configuration : 1691 bytes
!
! Last configuration change at 13:47:06 UTC Fri Oct 18 2024
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname IOU3
!
boot-start-marker
boot-end-marker
!
!
logging discriminator EXCESS severity drops 6 msg-body drops EXCESSCOLL
logging buffered 50000
logging console discriminator EXCESS
!
no aaa new-model
!
!
!
!
!
no ip icmp rate-limit unreachable
!
!
!
no ip domain-lookup
no ip cef
no ipv6 cef
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
interface Ethernet0/0
 switchport access vlan 10
 switchport mode access
!
interface Ethernet0/1
 switchport access vlan 10
 switchport mode access
!
interface Ethernet0/2
 switchport access vlan 10
 switchport mode access
!
interface Ethernet0/3
 switchport access vlan 30
 switchport mode access
!
interface Ethernet1/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet1/1
!
interface Ethernet1/2
!
interface Ethernet1/3
!
interface Ethernet2/0
!
interface Ethernet2/1
!
interface Ethernet2/2
!
interface Ethernet2/3
!
interface Ethernet3/0
!
interface Ethernet3/1
!
interface Ethernet3/2
!
interface Ethernet3/3
!
interface Vlan1
 no ip address
 shutdown
!
ip forward-protocol nd
!
ip tcp synwait-time 5
ip http server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
!
!
!
!
control-plane
!
!
line con 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
line aux 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
line vty 0 4
 login
!
!
!
end
```

> N'oubliez pas les VLANs sur tous les switches.

🖥️ **VM `dhcp1.client1.tp4`**, déroulez la [Checklist VM Linux](#checklist-vm-linux) dessus

🌞  **Mettre en place un serveur DHCP dans le nouveau bâtiment**

```
[lionel@dhcp ~]$ sudo cat /etc/dhcp/dhcpd.conf
[sudo] password for lionel:
option domain-name-servers 1.1.1.1;
default-lease-time 600;
max-lease-time 7200;
authoritative;
subnet 10.1.10.0 netmask 255.255.255.0 {
    # specify the range of lease IP address
    range dynamic-bootp 10.1.10.200 10.1.10.254;
    # specify broadcast address
    option broadcast-address 10.1.10.255;
    # specify gateway
    option routers 10.1.10.254;
}

PC3> ip dhcp
DDORA IP 10.1.10.204/24 GW 10.1.10.254

PC3> show ip

NAME        : PC3[1]
IP/MASK     : 10.1.10.204/24
GATEWAY     : 10.1.10.254
DNS         : 1.1.1.1
DHCP SERVER : 10.1.10.253
DHCP LEASE  : 352, 600/300/525
MAC         : 00:50:79:66:68:03
LPORT       : 20033
RHOST:PORT  : 127.0.0.1:20034
MTU         : 1500


```

🌞  **Vérification**
```
PC3> ping 1.1.1.1
84 bytes from 1.1.1.1 icmp_seq=1 ttl=53 time=30.232 ms
84 bytes from 1.1.1.1 icmp_seq=2 ttl=53 time=23.014 ms

PC3> ping ynov.com
ynov.com resolved to 104.26.11.233

84 bytes from 104.26.11.233 icmp_seq=1 ttl=53 time=28.574 ms
84 bytes from 104.26.11.233 icmp_seq=2 ttl=53 time=25.977 ms

![DC Archi](../img/dc_archi.png)
