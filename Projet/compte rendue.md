# Compte rendue

## Somaire
1. Ajout d'un utilisateur
2. Suppression de la connexion SSH root
3. Changement du mot de passe root
4. Utilisation des clés SSH
5. Ajout d'une user proxmox
6. allocation de l'espace
---
### 1. Ajout d'un utilisateur


```zsh
adduser nico

root@hv1-michaux:~# usermod -aG sudo nico

```

Mot de passe de l'utilisateur en MP Discord si tu veux. car tu peux comprendre que je ne le mets pas là :)

### 2. Suppression de la connexion SSH root

```zsh
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

Maintenant, les connexions SSH avec root ne marchent plus. maintenant il faut se co avec l'utilisateur Nico

### 3. Changement du mot de passe root

```zsh
root@hv1-michaux:~# passwd

csGOleGOat41

```

### 4. Utilisation des clés SSH


![clef](https://media.tenor.com/3r0eT--shu0AAAAM/ewan-mewing.gif)
```zsh
nico@hv1-michaux:~$ ssh-keygen


┌─[✗]─[nico@parrot]─[~]
└──╼ $ssh-keygen


┌─[nico@parrot]─[~]
└──╼ $ssh-copy-id -i ~/.ssh/id_rsa.pub -p 2222 nico@wilfart.fr

```

La connexion SSH par clé fonctionne désormais.

### 5. Ajout d'une user proxmox

```zsh
root@hv1-michaux:/home/nico# nano /etc/pve/user.cfg 


  GNU nano 7.2                    /etc/pve/user.cfg                             
user:root@pam:1:0:::mail@mail.com::
user:nico@pam:1:0:::maisouicestclaire@mail.com::

```
  
![perm](/image/image.png)


### 6. allocation de l'espace