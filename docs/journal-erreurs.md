# Journal des erreurs rencontrées

## 1. Fichier de config Apache contenant du HTML
**Erreur** : `Syntax error: Expected </h1>Bienvenue> but saw </body>`
**Cause** : Le contenu HTML avait été mis directement dans le fichier .conf Apache.
**Solution** : Séparer le contenu HTML dans /var/www/html/index.html et garder uniquement les directives Apache dans le fichier .conf.

## 2. ssh-copy-id inexistant sur Windows
**Erreur** : `ssh-copy-id : Le terme n'est pas reconnu`
**Cause** : La commande ssh-copy-id n'existe pas sur Windows.
**Solution** : Copier manuellement la clé publique dans ~/.ssh/authorized_keys sur le serveur.

## 3. Page Apache par défaut au lieu de MediaWiki
**Erreur** : Page "It works!" affichée
**Cause** : index.html prenait priorité sur index.php.
**Solution** : Supprimer /var/www/html/index.html.

## 4. Erreur MIME type sur load.php
**Erreur** : `MIME type mismatch bloquant CSS et JS`
**Cause** : $wgScriptPath = "/mediawiki" ne correspondait pas à la structure réelle des fichiers.
**Solution** : Changer $wgScriptPath = "" dans LocalSettings.php.
