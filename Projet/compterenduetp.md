# COMPTE RENDUE TP

## SOMMAIRE
0. update upgrade
1. CHANGEMENT MOT DE PASSE ET CREATION USER
2. DESACTIVATION ROOT SSH
3. CLEF SSH
4. INSTALLATION NGINX
5. INSTALLATION FAILBAN
6. INSTALLATION UFW

### 0. update upgrade

apt update
apt upgrade
apt-get update
apt-get upgrade


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

### 5. INSTALLATION FAILBAN

```zsh
root@debian:/home/nico# apt install fail2ban
root@debian:/home/nico# apt install rsyslog

```

```zsh
[sshd]


port    = ssh
logpath = /var/log/auth.log
backend = %(sshd_backend)s
```

```zsh
root@debian:/home/nico# systemctl restart fail2ban
root@debian:/home/nico# systemctl status fail2ban
● fail2ban.service - Fail2Ban Service
     Loaded: loaded (/lib/systemd/system/fail2ban.service; enabled; preset: enabled)
     Active: active (running) since Mon 2024-12-16 10:41:05 CET; 6s ago
```

### 6. INSTALLATION UFW

```zsh

root@debian:/var/log# apt-get install ufw

root@debian:/var/log# systemctl status ufw
● ufw.service - Uncomplicated firewall
     Loaded: loaded (/lib/systemd/system/ufw.service; enabled; preset: enabled)
     Active: active (exited) since Mon 2024-12-16 10:52:20 CET; 1s ago
```

```zsh

root@debian:/var/log#  ufw allow ssh
root@debian:/var/log#  ufw allow 22
root@debian:/var/log#  ufw allow 'Nginx HTTP'

root@debian:/var/log#  ufw enable
```

```zsh
root@debian:/var/log#  ufw status verbose
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere                  
22                         ALLOW IN    Anywhere                  
80/tcp (Nginx HTTP)        ALLOW IN    Anywhere                  
22/tcp (v6)                ALLOW IN    Anywhere (v6)             
22 (v6)                    ALLOW IN    Anywhere (v6)             
80/tcp (Nginx HTTP (v6))   ALLOW IN    Anywhere (v6)
```


### 7. Rsync

```zsh
root@debian:/home# apt install rsync

root@debian:/home# mkdir backup
root@debian:/home# ls
backup	deploy	nico
root@debian:/home# rsync -avz /var/www/html /var/log /home/backup/

```