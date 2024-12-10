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





```zsh
root@hv1-michaux:/home/nico# apt-get install lvm2

```

j'ai eu un probleme de PATCH donc voila les commande efectuer pour debug.  

```zsh
root@hv1-michaux:/home/nico# echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
root@hv1-michaux:/home/nico# export PATH=$PATH:/sbin
```

Ensuite j'ai pu repredre le travail

```zsh
root@hv1-michaux:/home/nico# pvcreate /dev/sdb
  Physical volume "/dev/sdb" successfully created.
root@hv1-michaux:/home/nico# pvcreate /dev/sdc
  Physical volume "/dev/sdc" successfully created.
```

```zsh
root@hv1-michaux:/home/nico# pvdisplay


  "/dev/sdb" is a new physical volume of "100.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdb
  VG Name               
  PV Size               100.00 GiB
  Allocatable           NO
  PE Size               0   
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               KDZvCa-sQSp-05nG-SJOg-iKLM-Ls9L-UvQZN8
   
  "/dev/sdc" is a new physical volume of "100.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdc
  VG Name               
  PV Size               100.00 GiB
  Allocatable           NO
  PE Size               0   
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               bG8K56-eo57-hhtx-sWLj-IHT9-JgUE-4bWpqR

```

```zsh
root@hv1-michaux:/home/nico# vgcreate VG01 /dev/sdb
  Volume group "VG01" successfully created
root@hv1-michaux:/home/nico# vgcreate VG02 /dev/sdc
  Volume group "VG02" successfully created

root@hv1-michaux:/home/nico# vgdisplay
  --- Volume group ---
  VG Name               VG02
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <100.00 GiB
  PE Size               4.00 MiB
  Total PE              25599
  Alloc PE / Size       0 / 0   
  Free  PE / Size       25599 / <100.00 GiB
  VG UUID               3xyFqF-x58k-ZBcM-oyww-5wBa-X2Ms-Y6iutN
   
  --- Volume group ---
  VG Name               VG01
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <100.00 GiB
  PE Size               4.00 MiB
  Total PE              25599
  Alloc PE / Size       0 / 0   
  Free  PE / Size       25599 / <100.00 GiB
  VG UUID               89f8Os-aRj7-8hTw-8SIx-WBpV-q4iD-zl6c47

```

```zsh
root@hv1-michaux:/home/nico# lvcreate -n VPN -L 30g VG01
  Logical volume "VPN" created.
root@hv1-michaux:/home/nico# lvcreate -n Gestionnaire -L 10g VG01
  Logical volume "Gestionnaire" created.
root@hv1-michaux:/home/nico# lvcreate -n Transfere -L 50g VG01
  Logical volume "Transfere" created.

root@hv1-michaux:/home/nico# ls -al /dev/VG01/
total 0
drwxr-xr-x  2 root root  100 Dec 10 10:34 .
drwxr-xr-x 20 root root 4360 Dec 10 10:34 ..
lrwxrwxrwx  1 root root    7 Dec 10 10:33 Gestionnaire -> ../dm-7
lrwxrwxrwx  1 root root    7 Dec 10 10:34 Transfere -> ../dm-8
lrwxrwxrwx  1 root root    7 Dec 10 10:33 VPN -> ../dm-6
```

```zsh
root@hv1-michaux:/home/nico# mkfs -t ext4 /dev/VG01/VPN
mke2fs 1.47.0 (5-Feb-2023)
Discarding device blocks: done                            
Creating filesystem with 7864320 4k blocks and 1966080 inodes
Filesystem UUID: cdd79720-6a0c-42b3-be7c-8d4af5b551f3
Superblock backups stored on blocks: 
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
	4096000

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done 

root@hv1-michaux:/home/nico# mkfs -t ext4 /dev/VG01/Gestionnaire
...

root@hv1-michaux:/home/nico# mkfs -t ext4 /dev/VG01/Transfere 
...

```


```zsh
root@hv1-michaux:/# mkdir /mnt/Gestionnaire
root@hv1-michaux:/# mount /dev/mapper/VG01-Gestionnaire /mnt/Gestionnaire
root@hv1-michaux:/# mkdir /mnt/Transfere
root@hv1-michaux:/# mount /dev/mapper/VG01-Transfere /mnt/Transfere/
root@hv1-michaux:/# mkdir /mnt/VPN
root@hv1-michaux:/# mount /dev/mapper/VG01-VPN /mnt/VPN/

```

vérification

```zsh
root@hv1-michaux:/# lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda                    8:0    0   20G  0 disk 
|-sda1                 8:1    0 1007K  0 part 
|-sda2                 8:2    0  512M  0 part 
`-sda3                 8:3    0 19.5G  0 part 
  |-pve-swap         252:0    0  2.4G  0 lvm  [SWAP]
  |-pve-root         252:1    0  8.6G  0 lvm  /
  |-pve-data_tmeta   252:2    0    1G  0 lvm  
  | `-pve-data-tpool 252:4    0  6.6G  0 lvm  
  |   `-pve-data     252:5    0  6.6G  1 lvm  
  `-pve-data_tdata   252:3    0  6.6G  0 lvm  
    `-pve-data-tpool 252:4    0  6.6G  0 lvm  
      `-pve-data     252:5    0  6.6G  1 lvm  
sdb                    8:16   0  100G  0 disk 
|-VG01-VPN           252:6    0   30G  0 lvm  /mnt/VPN
|-VG01-Gestionnaire  252:7    0   10G  0 lvm  /mnt/Gestionnaire
`-VG01-Transfere     252:8    0   50G  0 lvm  /mnt/Transfere
sdc                    8:32   0  100G  0 disk 
`-VG02-Backup        252:9    0   60G  0 lvm  /backup
sr0                   11:0    1  1.3G  0 rom  

```