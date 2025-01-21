# III. Docker compose

Pour la fin de ce TP on va manipuler un peu `docker compose`.

🌞 **Créez un fichier `docker-compose.yml`**

```bash
[lionel@localhost ~]$ sudo cat compose_test/docker-compose.yml
version: "3"

services:
  conteneur_nul:
    image: debian
    entrypoint: sleep 9999
  conteneur_flopesque:
    image: debian
    entrypoint: sleep 9999
```

🌞 **Lancez les deux conteneurs** avec `docker compose`

```
[lionel@localhost ~]$ cd compose_test/
[lionel@localhost compose_test]$ docker compose up -d
WARN[0000] /home/lionel/compose_test/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
[+] Running 3/3
 ✔ conteneur_flopesque Pulled                                                       2.8s
   ✔ fdf894e782a2 Already exists                                                    0.0s
 ✔ conteneur_nul Pulled                                                             3.1s
[+] Running 3/3
 ✔ Network compose_test_default                  Created                            0.9s
 ✔ Container compose_test-conteneur_nul-1        Started                            0.6s
 ✔ Container compose_test-conteneur_flopesque-1  Started                            0.6s
 ```

🌞 **Vérifier que les deux conteneurs tournent**
```
[lionel@localhost compose_test]$ docker compose ps
WARN[0000] /home/lionel/compose_test/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
NAME                                 IMAGE     COMMAND        SERVICE               CREATED          STATUS          PORTS
compose_test-conteneur_flopesque-1   debian    "sleep 9999"   conteneur_flopesque   56 seconds ago   Up 55 seconds
compose_test-conteneur_nul-1         debian    "sleep 9999"   conteneur_nul         56 seconds ago   Up 55 seconds
```
🌞 **Pop un shell dans le conteneur `conteneur_nul`**

```
root@5f1a3dbe2406:/# ping conteneur_flopesque
PING conteneur_flopesque (172.18.0.3) 56(84) bytes of data.
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.3): icmp_seq=1 ttl=64 time=0.116 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.3): icmp_seq=2 ttl=64 time=0.073 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.3): icmp_seq=3 ttl=64 time=0.066 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.3): icmp_seq=4 ttl=64 time=0.076 ms
```

