# I. Init


## 1. Installation de Docker


## 2. Vérifier que Docker est bien là



## 3. sudo c pa bo

🌞 **Ajouter votre utilisateur au groupe `docker`**
```
[lionel@localhost ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

## 4. Un premier conteneur en vif


🌞 **Lancer un conteneur NGINX**
```
[lionel@localhost ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS
     NAMES
e814b9a32e56   nginx     "/docker-entrypoint.…"   39 seconds ago   Up 38 seconds   0.0.0.0:9999->80/tcp, [::]:9999->80/tcp   romantic_feistel
```

🌞 **Visitons**

```
[lionel@localhost ~]$ curl 10.3.1.11:9999
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

```

🌞 **On va ajouter un site Web au conteneur NGINX**

```
[lionel@localhost nginx]$ docker run -d -p 9999:8080 -v /home/lionel/nginx/index.html:/var/www/html/index.html -v /home/lionel/nginx/site_nul.conf:/etc/nginx/conf.d/site_nul.conf nginx
cd39974da5d077cdeac34e77f3db4ff93fffd72e571a62616b543d887cd3a27f
[lionel@localhost nginx]$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS
                 NAMES
cd39974da5d0   nginx     "/docker-entrypoint.…"   11 seconds ago   Up 11 seconds   80/tcp, 0.0.0.0:9999->8080/tcp, [::]:9999->8080/tcp   clever_brahmagupta
```

🌞 **Visitons**

```
[lionel@localhost nginx]$ curl 10.3.1.11:9999
<h1>MEOOOW</h1>
```

🌞 **Lance un conteneur Python, avec un shell**

```
[lionel@localhost nginx]$ docker run -it python bash
Unable to find image 'python:latest' locally
latest: Pulling from library/python
fdf894e782a2: Pull complete
5bd71677db44: Pull complete
551df7f94f9c: Pull complete
ce82e98d553d: Pull complete
5f0e19c475d6: Pull complete
abab87fa45d0: Pull complete
2ac2596c631f: Pull complete
Digest: sha256:220d07595f288567bbf07883576f6591dad77d824dce74f0c73850e129fa1f46
Status: Downloaded newer image for python:latest
root@3dba1aa763a0:/#
```

🌞 **Installe des libs Python**

```
Python 3.13.1 (main, Dec  4 2024, 20:40:27) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import aiohttp
```


