# TP1 : Maîtrise réseau du votre poste

# I. Basics

☀️ **Carte réseau WiFi**
```
commande :

ipconfig /all 

- l'adresse MAC de votre carte WiFi

  Adresse physique . . . . . . . . . . . : CC-5E-F8-6E-4B-33

- l'adresse IP de votre carte WiFi

   Adresse IPv4. . . . . . . . . . . . . .: 10.33.73.115(préféré)

- le masque de sous-réseau du réseau LAN auquel vous êtes connectés en WiFi

  Masque de sous-réseau. . . . . . . . . : 255.255.240.0

  - en notation CIDR, par exemple `/16`
  
   10.33.73.115/20
---
```

☀️ **Déso pas déso**
```

- l'adresse de réseau du LAN auquel vous êtes connectés en WiFi

  10.33.64.0

- l'adresse de broadcast

 10.33.79.255

- le nombre d'adresses IP disponibles dans ce réseau

  4094
```


☀️ **Hostname**
```
C:\Users\lione>hostname
Lionelp
```

☀️ **Passerelle du réseau**
```
- l'adresse IP de la passerelle du réseau

 ipconfig /all
 Passerelle par défaut. . . . . . . . . : 10.33.79.254

- l'adresse MAC de la passerelle du réseau

 arp -a
 10.33.79.254          7c-5a-1c-d3-d8-76     dynamique

```
☀️ **Serveur DHCP et DNS**
```
- l'adresse IP du serveur DHCP qui vous a filé une IP

  ipconfig /all
  Serveur DHCP . . . . . . . . . . . . . : 10.33.79.254

- l'adresse IP du serveur DNS que vous utilisez quand vous allez sur internet
 195.7.117.146
```

☀️ **Table de routage**
```
- dans votre table de routage, laquelle est la route par défaut
route print 
       0.0.0.0          0.0.0.0     10.33.79.254     10.33.73.115     30
```


# II. Go further


☀️ **Hosts ?**
```
- faites en sorte que pour votre PC, le nom `b2.hello.vous` corresponde à l'IP `1.1.1.1`

PS C:\Windows\system32> cat C:\Windows\system32\drivers\etc\hosts
1.1.1.1 b2.hello.vous

- prouvez avec un `ping b2.hello.vous` que ça ping bien `1.1.1.1`

PS C:\Windows\system32> ping b2.hello.vous

Envoi d’une requête 'ping' sur b2.hello.vous [1.1.1.1] avec 32 octets de données :
Réponse de 1.1.1.1 : octets=32 temps=44 ms TTL=55
Réponse de 1.1.1.1 : octets=32 temps=19 ms TTL=55
Réponse de 1.1.1.1 : octets=32 temps=33 ms TTL=55



```

☀️ **Go mater une vidéo youtube et déterminer, pendant qu'elle tourne...**
```
- l'adresse IP du serveur auquel vous êtes connectés pour regarder la vidéo
91.68.245.77

- le port du serveur auquel vous êtes connectés
 443

- le port que votre PC a ouvert en local pour se connecter au port du serveur distant
 62331

```

☀️ **Requêtes DNS**

```

- à quelle adresse IP correspond le nom de domaine `www.thinkerview.com`

PS C:\Users\lione> nslookup www.thinkerview.com
Serveur :   dns.google
Address:  8.8.8.8

- à quel nom de domaine correspond l'IP `143.90.88.12`

PS C:\Users\lione> nslookup 143.90.88.12
Serveur :   dns.google
Address:  8.8.8.8

Nom :    EAOcf-140p12.ppp15.odn.ne.jp
Address:  143.90.88.12

```

☀️ **Hop hop hop**
```
Passe par 3 machines.

PS C:\Users\lione> nslookup www.ynov.com
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    www.ynov.com
Addresses:  2606:4700:20::ac43:4ae2
          2606:4700:20::681a:ae9
          2606:4700:20::681a:be9
          104.26.10.233
          172.67.74.226
          104.26.11.233
```

☀️ **IP publique**

```
ipconfig /all

Passerelle par défaut. . . . . . . . . : 10.33.79.254
```

# III. Le requin

☀️ **Capture ARP**
```PS C:\Users\lione> ping 10.33.79.254

Envoi d’une requête 'Ping'  10.33.79.254 avec 32 octets de données :
Délai d’attente de la demande dépassé.
Délai d’attente de la demande dépassé.
Délai d’attente de la demande dépassé.
Délai d’attente de la demande dépassé.

Statistiques Ping pour 10.33.79.254:
    Paquets : envoyés = 4, reçus = 0, perdus = 4 (perte 100%),
```

☀️ **Capture DNS**
```
PS C:\Users\lione> nslookup youtube.com
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    youtube.com
Addresses:  2a00:1450:4007:818::200e
          142.250.179.110
```


☀️ **Capture TCP**