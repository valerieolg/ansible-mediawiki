# Ce que ce projet m'a appris

## Ansible
- L'idempotence est un avantage majeur : relancer le playbook plusieurs fois ne cause pas de problèmes.
- Organiser le code en rôles séparés facilite le débogage et la réutilisation.
- Il faut toujours tester la connectivité SSH avant de lancer un playbook.

## Sécurité
- Ne jamais désactiver PasswordAuthentication avant d'avoir confirmé que la clé fonctionne.
- Le pare-feu nftables doit être configuré sur les deux serveurs, pas seulement le serveur web.
- Les fichiers sensibles (mots de passe, clés) ne doivent jamais être poussés sur GitHub.

## Infrastructure
- Séparer les services sur deux serveurs demande plus de configuration réseau mais prépare l'infrastructure à évoluer.
- L'ordre d'installation des dépendances est important : PHP doit être installé avant d'activer son module Apache.

## Débogage
- Toujours vérifier les logs Apache (/var/log/apache2/error.log) en cas de problème web.
- La commande nc -zv permet de tester rapidement si un port est accessible entre deux serveurs.
- Lire attentivement les messages d'erreur Ansible : ils indiquent exactement quelle tâche a échoué.
