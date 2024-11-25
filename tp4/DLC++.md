# DLC Ultime : "Le Défi du SysAdmin Suprême"

## Contexte
Un serveur compromis a laissé des fichiers éparpillés, des logs corrompus et des traces suspectes. Votre mission est de :
1. Analyser et nettoyer un fichier de logs corrompu.
2. Retrouver des informations dissimulées dans des fichiers cryptés ou masqués.
3. Automatiser des tâches complexes pour sécuriser et réorganiser le système.

**Attention** : Ce défi nécessite une maîtrise totale des outils de manipulation de texte et du scripting sous Linux.

---

## Étape 1 : Analyse et nettoyage d’un fichier de logs

### Objectif
Vous recevez un fichier de logs, `/var/log/messy_logs.log`, rempli de données inutiles, de doublons et de messages corrompus. Votre tâche est de le nettoyer et d’en extraire des informations précises.

1. **Supprimez toutes les lignes contenant les mots-clés `DEBUG` ou `TRACE`.**
2. **Conservez uniquement les logs des 3 derniers jours.**
3. **Comptez le nombre d’occurrences de chaque adresse IP présente dans les logs.**
4. **Créez un fichier `cleaned_logs.log` contenant les 100 premières lignes triées par timestamp.**

---

## Étape 2 : Retrouver des données dissimulées

### Objectif
Plusieurs fichiers dans `/home/hidden_data` contiennent des informations importantes, mais elles sont soit masquées, soit cryptées. Votre mission est de retrouver ces données.

1. **Recherchez dans tous les fichiers du répertoire `/home/hidden_data` les lignes contenant une adresse e-mail valide.**
2. **Extrayez les adresses IP cachées dans les fichiers contenant le mot-clé `IMPORTANT`.**
3. **Concaténez toutes les lignes contenant des informations en base64, décodez-les, et sauvegardez-les dans `decoded_data.txt`.**
4. **Identifiez tous les fichiers contenant le mot `SECRET` en ignorant la casse et sauvegardez leur chemin dans `secret_files.txt`.**

---

## Étape 3 : Automatisation avancée avec un script

### Objectif
Créez un script complet nommé `system_cleaner.sh` pour automatiser les tâches suivantes :

1. **Surveiller et supprimer automatiquement les fichiers plus vieux que 7 jours dans `/tmp`.**
2. **Analyser les logs système (`/var/log/syslog`) pour détecter des échecs de connexion SSH, puis bloquer les IP correspondantes avec `firewalld`.**
3. **Créer un rapport de toutes les modifications apportées au répertoire `/etc` dans les 24 dernières heures.**

Le script doit :
- Être entièrement automatisé.
- Loguer toutes ses actions dans `/var/log/system_cleaner.log`.

---

## Étape 4 : Le puzzle final

### Objectif
Dans le fichier `/opt/challenge/puzzle_data.txt`, des instructions sont cachées sous forme de fragments éparpillés dans tout le fichier. Votre mission est de :

1. **Assembler les lignes contenant le mot-clé `FRAGMENT` (dans un ordre précis basé sur un timestamp à la fin de chaque ligne).**
2. **Rechercher dans le fichier reconstitué une séquence hexadécimale (exemple : `0xdeadbeef`) et convertir cette séquence en texte ASCII.**
3. **Le mot obtenu est votre mot de passe final.**

---