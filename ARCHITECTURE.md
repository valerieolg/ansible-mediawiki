# Architecture du projet

## Machines virtuelles

| Nom | IP | Rôle |
|-----|----|------|
| debian-serveur | 192.168.0.115 | Nœud de contrôle Ansible |
| debian-serveur-web | 192.168.0.114 | Apache + PHP + MediaWiki |
| debian-serveur-db | 192.168.0.148 | MariaDB |

## Ports ouverts

| Serveur | Port | Service |
|---------|------|---------|
| srv-web | 22 | SSH |
| srv-web | 80 | HTTP |
| srv-web | 443 | HTTPS |
| srv-db | 22 | SSH |
| srv-db | 3306 | MariaDB (depuis srv-web uniquement) |

## Flux réseau

- Ansible control → SSH → srv-web (port 22)
- Ansible control → SSH → srv-db (port 22)
- Navigateur → HTTP → srv-web (port 80)
- srv-web → SQL → srv-db (port 3306)

## Frontières de confiance

- srv-db refuse toute connexion MariaDB sauf depuis 192.168.0.114
- srv-web refuse tous les ports sauf 22, 80 et 443
- Authentification SSH par clé ed25519 uniquement, root désactivé
