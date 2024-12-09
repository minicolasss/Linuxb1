# Compte rendue

## Somaire

## ajout utilisateur


```
adduser nico

root@hv1-michaux:~# usermod -aG sudo nico

```

mot de passe de l'user en mp discord si tu veux. car tu peut comprendre que je ne le mete pas là :)

## suppretion de la connection ssh root

```
root@hv1-michaux:~# nano /etc/ssh/sshd_config

...
#LoginGraceTime 2m
PermitRootLogin no
#StrictModes yes
#MaxAuthTries 6
#MaxSessions 10
...

root@hv1-michaux:~# /etc/init.d/ssh restart
```

mientement les connection ssh avec root ne marche plus. mientement il faut ce co avec user nico    

changement de mot de passe de root

```
root@hv1-michaux:~# passwd

csGOleGOat41

```

## les clef ssh enfin


```
nico@hv1-michaux:~$ ssh-keygen


┌─[✗]─[nico@parrot]─[~]
└──╼ $ssh-keygen


┌─[nico@parrot]─[~]
└──╼ $ssh-copy-id -i ~/.ssh/id_rsa.pub -p 2222 nico@wilfart.fr

```

mientement la connection par clef marche.