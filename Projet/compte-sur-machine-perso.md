# Compte rendue

## Somaire
1. installation de Proxmox
2. mise en place de la premier VM
3. ouverture du ports 

---
### 1. installation de Proxmox

Sur mon serveur j'ai installé Proxmox avec une clef bootable.

J'ai rencontré 2 problèmes durant l'installation.

le premier est que je n'avais pas activé la virtualisation sur le serveur 
Donc j'ai activé la virtualisation sur le BIOS et j'ai refait une installation.  
Le 2ème problème rencontré est que le serveur ne veut pas booter sur le bon disque, donc pour booter le serveur je suis obligé de le booter avec l'option debug de ma clef bootable.

### 2. mise en place de la premier VM

dans un premier temps, j'ai ajouté des fichiers ISO sur le serveur pour mes prochaines VM

![iso](./image/iso.png)

ensuite j'ai créer ma premier VM 

![VM1er](./image/VM1er.png)

### 3. ouverture du ports 

Sur le pannel Admin de ma Freebox j'ai créer une configuration pour mon serveur j'ai ouvert le port 20457 et ma box qui le redirige sur le port 8006 de mon serveur.