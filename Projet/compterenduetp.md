# COMPTE RENDUE TP

## SOMMAIRE
1. CHANGEMENT MOT DE PASSE ET CREATION USER
2. DESACTIVATION ROOT SSH
3. CLEF SSH
4. INSTALLATION NGINX

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

```zsh
nico@debian:~$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/nico/.ssh/id_rsa): 


┌─[nico@parrot]─[~]
└──╼ $ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/nico/.ssh/id_rsa): 


┌─[✗]─[nico@parrot]─[~]
└──╼ $ssh-copy-id -p 6220  nico@wilfart.fr 
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed


```

### 4. INSTALLATION NGINX

```zsh
root@debian:/home/nico# apt install nginx
```

vérification :

```bash
root@debian:/home/nico# systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; preset: enable>
     Active: active (running) since Mon 2024-12-16 10:24:06 CET; 1min 37s ago
```