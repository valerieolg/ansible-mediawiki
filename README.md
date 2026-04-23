# Déploiement automatisé de MediaWiki avec Ansible

## Contexte

Dans le cadre d'un projet DevOps pour GreenOps, une coopérative spécialisée
dans la cartographie de cavistes de vin naturel, nous avons déployé un wiki
interne basé sur MediaWiki. L'objectif était d'automatiser entièrement
l'installation via Ansible selon le principe d'Infrastructure as Code (IaC),
permettant un déploiement reproductible en une seule commande.

## Architecture

- **Machine de contrôle Ansible** → 192.168.0.115 (debian-serveur)
- **Serveur web** → 192.168.0.114 (debian-serveur-web) : Apache2 + PHP + MediaWiki
- **Serveur DB** → 192.168.0.148 (debian-serveur-db) : MariaDB

## Prérequis

- 3 machines Debian 12 avec accès root
- Ansible installé sur la machine de contrôle
- Connectivité SSH entre les machines (clés ed25519)

## Structure du projet

<img width="238" height="276" alt="image" src="https://github.com/user-attachments/assets/b60772d4-35df-4526-8abe-8604447fc5b8" />


## Déploiement

```bash
# 1. Cloner le dépôt
git clone https://github.com/VOTRE_USERNAME/ansible-mediawiki.git
cd ansible-mediawiki

# 2. Configurer l'inventaire
nano inventory.ini  # Adapter les IPs

# 3. Lancer le déploiement
ansible-playbook -i inventory.ini site.yml
```

## Erreurs rencontrées et solutions

### 1. Permission denied sur apt
**Erreur** : `Failed to lock apt for exclusive operation: Permission denied`  
**Cause** : Ansible n'avait pas les droits root.  
**Solution** : Ajouter `become: yes` dans `site.yml`.

### 2. Handler introuvable
**Erreur** : `The requested handler 'Recharger Apache' was not found`  
**Cause** : Le handler était référencé mais jamais défini.  
**Solution** : Remplacer le `notify` par une tâche `service` directe dans le rôle.

### 3. Page Apache par défaut affichée
**Erreur** : Page "It works!" au lieu de MediaWiki  
**Cause** : `index.html` prenait priorité sur `index.php`.  
**Solution** : Supprimer `/var/www/html/index.html`.

### 4. Erreur MIME type sur load.php
**Erreur** : `MIME type mismatch` bloquant CSS et JS  
**Cause** : `$wgScriptPath = "/mediawiki"` ne correspondait pas à la structure réelle.  
**Solution** : Changer `$wgScriptPath = ""` dans `LocalSettings.php`.

## Résultat

MediaWiki est accessible à l'adresse `http://192.168.0.114` et déployable entièrement en une seule commande Ansible.
