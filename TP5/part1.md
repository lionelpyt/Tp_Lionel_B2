# I. Tester

🌞 **Récupérer l'application dans la VM `hosting.tp5.b1`**
```

```

🌞 **Essayer de lancer l'app**
```
[lionel@hosting ~]$ python server.py

[lionel@hosting ~]$ ss -alunpt | grep python
tcp   LISTEN 0      1            0.0.0.0:13337      0.0.0.0:*    users:(("python",pid=1592,fd=3))
```

🌞 **Tester l'app depuis `hosting.tp5.b1`**
```
[lionel@hosting ~]$ python client.py
Calcul à envoyer: 500*500
250000

[lionel@hosting ~]$ python server.py
Données reçues du client : b'Hello'
```