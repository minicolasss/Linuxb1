# COMPTE RENDUE TP

## SOMMAIRE
1. CHANGEMENT MOT DE PASSE ET CREATION USER
2. DESACTIVATION ROOT SSH
3. CLEF SSH

### 1. CHANGEMENT MOT DE PASSE ET CREATION USER


```zsh
root@debian:~# passwd
mot de passe LaVie 

root@debian:~# adduser nico
root@debian:~# usermod -aG sudo nico          
root@debian:~# groups nico          
nico : nico sudo users
mot de passe NicoLaVie
```



### 2. DESACTIVATION ROOT SSH

```zsh
root@debian:~# nano /etc/ssh/sshd_config


...
PermitRootLogin no 
...

root@debian:~# systemctl restart sshd


```
test
```
┌─[nico@parrot]─[~]
└──╼ $ssh root@wilfart.fr -p6220
root@wilfart.fr's password: 
Permission denied, please try again.
root@wilfart.fr's password: 
```


### 3. CLEF SSH