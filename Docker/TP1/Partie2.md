# II. Images

## 1. Images publiques

🌞 **Récupérez des images**
```
[lionel@localhost nginx]$ docker images
REPOSITORY           TAG       IMAGE ID       CREATED         SIZE
linuxserver/wikijs   latest    863e49d2e56c   5 days ago      465MB
python               latest    3ca4060004b1   7 days ago      1.02GB
python               3.11      342f2c43d207   7 days ago      1.01GB
nginx                latest    66f8bdd3810c   2 weeks ago     192MB
wordpress            latest    c89b40a25cd1   2 weeks ago     700MB
mysql                5.7       5107333e08a8   12 months ago   501MB
hello-world          latest    d2c94e258dcb   19 months ago   13.3kB
```

🌞 **Lancez un conteneur à partir de l'image Python**
```
[lionel@localhost nginx]$ docker run -it python:3.11 bash
root@d4e28d31f103:/#
```
## 2. Construire une image

Pour construire une image il faut :

- créer un fichier `Dockerfile`
- exécuter une commande `docker build` pour produire une image à partir du `Dockerfile`

🌞 **Ecrire un Dockerfile pour une image qui héberge une application Python**
```
[lionel@localhost python_app_build]$ sudo cat Dockerfile
# Utiliser une image Debian de base
FROM debian

# Mettre à jour la liste des paquets et installer Python
RUN apt update -y
RUN apt install -y python3
# Installer la librairie Python "emoji"
RUN apt install -y python3-emoji

# Copier l'application dans le conteneur
COPY app.py /app/app.py

# Définir le point d'entrée de l'application
ENTRYPOINT ["python3", "app.py"]
```

🌞 **Build l'image**

```
[lionel@localhost python_app_build]$ docker build . -t python_app:version_de_ouf
[+] Building 0.5s (10/10) FINISHED                                                                       docker:default
 => [internal] load build definition from Dockerfile                                                               0.0s
 => => transferring dockerfile: 472B                                                                               0.0s
 => [internal] load metadata for docker.io/library/debian:latest                                                   0.4s
 => [internal] load .dockerignore                                                                                  0.0s
 => => transferring context: 2B                                                                                    0.0s
 => [1/5] FROM docker.io/library/debian:latest@sha256:17122fe3d66916e55c0cbd5bbf54bb3f87b3582f4d86a755a0fd3498d36  0.0s
 => [internal] load build context                                                                                  0.0s
 => => transferring context: 86B                                                                                   0.0s
 => CACHED [2/5] RUN apt update -y                                                                                 0.0s
 => CACHED [3/5] RUN apt install -y python3                                                                        0.0s
 => CACHED [4/5] RUN apt install -y python3-emoji                                                                  0.0s
 => CACHED [5/5] COPY app.py /app/app.py                                                                           0.0s
 => exporting to image                                                                                             0.0s
 => => exporting layers                                                                                            0.0s
 => => writing image sha256:4eaf43350f3bf63d25a3148d9e2c5294896ad0f6a4126726ba64f39b014e3edb                       0.0s
 => => naming to docker.io/library/python_app:version_de_ouf                                                       0.0s
 ```

🌞 **Lancer l'image**

```bash
[lionel@localhost python_app_build]$ docker run python_app:version_de_ouf
Cet exemple d'application est vraiment naze 👎
```
