# DLC : "L'ultime défi du SysAdmin"

## Contexte
Félicitations ! Vous avez restauré le serveur, mais les attaquants semblent avoir laissé des portes dérobées (backdoors) et d'autres vulnérabilités. Votre nouvelle mission est d'identifier ces failles et d’ajouter des couches supplémentaires de protection.

---

### Étape 1 : Analyse avancée et suppression des traces suspectes

1. **Rechercher des utilisateurs récemment ajoutés :**
   - Identifiez les utilisateurs ajoutés récemment en inspectant les logs de sécurité.
     ```bash
     sudo grep "new user" /var/log/secure
     ```
```zsh
[root@vbox ~]# grep -a "new user" /var/log/secure
Nov 24 18:11:10 vbox useradd[1450]: new user: name=attacker, UID=1000, GID=1000, home=/home/attacker, shell=/bin/bash, from=/dev/pts/0
Nov 25 19:19:01 vbox useradd[5121]: new user: name=attacker, UID=1000, GID=1000, home=/home/attacker, shell=/bin/bash, from=/dev/pts/0
```
2. **Trouver les fichiers récemment modifiés dans des répertoires critiques :**
   - Analysez les fichiers modifiés dans `/etc`, `/usr/local/bin` ou `/var` au cours des 7 derniers jours :
     ```bash
     sudo find /etc /usr/local/bin /var -type f -mtime -7
     ```
```zsh
[root@vbox ~]# sudo find /etc /usr/local/bin /var -type f -mtime -7
```
3. **Lister les services suspects activés :**
   - Vérifiez tous les services activés au démarrage pour identifier ceux qui ne devraient pas être présents :
     ```bash
     sudo systemctl list-unit-files --state=enabled
     ```
```zsh
[root@vbox ~]# sudo systemctl list-unit-files --state=enabled
```
4. **Supprimer une tâche cron suspecte :**
   - Identifiez et supprimez une tâche cron malveillante ajoutée à un utilisateur spécifique, par exemple `attacker` :
     ```bash
     sudo crontab -u attacker -r
     ```
```zsh
[root@vbox ~]# sudo crontab -u attacker -r
```
---

## Étape 2 : Configuration avancée de LVM

1. **Créer un snapshot du volume logique** :
   - Prenez un snapshot du volume logique `secure_data` pour sécuriser les données actuelles.
```zsh
[root@vbox ~]# lvcreate -L 200MB -s -n sec_data_snap /dev/vg_secure/secure_data 
  Logical volume "sec_data_snap" created.
```
2. **Tester le snapshot** :
   - Montez le snapshot et vérifiez qu’il contient bien les données actuelles.
```zsh
[root@vbox ~]# lvconvert --mergesnapshot /dev/vg_secure/sec_data_snap 
  Merging of volume vg_secure/sec_data_snap started.
  vg_secure/secure_data: Merged: 100.00%
[root@vbox ~]# mount /mnt/secure_data/
```
3. **Simuler une restauration** :
   - Supprimez un fichier dans `/mnt/secure_data`, puis restaurez-le à partir du snapshot.
```zsh
[root@vbox ~]# lvconvert --mergesnapshot /dev/vg_secure/sec_data_snap 
  Merging of volume vg_secure/sec_data_snap started.
  vg_secure/secure_data: Merged: 100.00%
[root@vbox ~]# mount /mnt/secure_data/
```
---

## Étape 3 : Renforcement du pare-feu avec des règles dynamiques

1. **Bloquer les attaques par force brute** :
   - Configurez `firewalld` pour détecter et bloquer automatiquement les connexions répétées sur SSH.
```zsh
[root@vbox ~]# yum install epel-release -y
[root@vbox ~]# yum install fail2ban -y

[root@vbox ~]# systemctl start fail2ban
systemctl enable fail2ban

[root@vbox ~]# sudo cp /etc/fail2ban/fail2ban.conf /etc/fail2ban/fail2ban.local
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local

```
2. **Restreindre l’accès SSH à une plage IP spécifique** :
   - Autorisez uniquement les connexions SSH depuis votre réseau local (192.168.x.x).

3. **Créer une zone sécurisée pour un service web** :
   - Configurez `firewalld` pour que seul HTTP/HTTPS soit autorisé dans une zone dédiée, et attribuez cette zone au serveur.

---

## Étape 4 : Création d'un script de surveillance avancé

1. **Écrivez un script `monitor.sh`** :
   - Surveille en temps réel :
     - Les connexions actives (`ss` ou `netstat`).
     - Les fichiers modifiés dans `/etc`.
   - Log les informations importantes dans `/var/log/monitor.log`.

2. **Ajoutez une alerte par e-mail** :
   - Configurez le script pour envoyer un e-mail lorsqu’un fichier dans `/etc` est modifié.

3. **Automatisez le script** :
   - Ajoutez une tâche cron pour exécuter le script toutes les 5 minutes.

---

## Étape 5 : Mise en place d’un IDS (Intrusion Detection System)

1. **Installer et configurer `AIDE`** :
   - Installez l’outil `AIDE` (Advanced Intrusion Detection Environment).
   - Initialisez la base de données.
   - Configurez `AIDE` pour surveiller les fichiers critiques du système.

2. **Tester `AIDE`** :
   - Faites une modification dans un fichier sous `/etc`.
   - Lancez une vérification avec `AIDE` et observez le rapport.

---