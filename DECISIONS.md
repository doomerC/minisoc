# Décisions

Journal des décisions de cadrage prises jusqu'au 21/11/2026. Les points non décidés sont indiqués comme tels.

## Décisions prises

| Sujet | Décision | Raison |
|---|---|---|
| Hébergement du projet | Dépôt GitHub, envoyé à l'encadrant | Demande de l'encadrant |
| Structure du dépôt | À plat : `app.py`, `db.py`, `parsers.py`, `rules.py`, plus `data/`, `docs/`, `exports/`, `screenshots/`, `tests/` | Simple à expliquer en démonstration ; structure inspirée de l'exemple `gaston-dev-web/mini-siem` (aucun code copié) |
| Jeux de logs SSH | Deux jeux synthétiques : `ssh_reference.log` (aucune alerte) et `ssh_signals.log` (scénarios S1 à S5) | Exigence du cahier : un jeu de référence et un jeu de signaux |
| Format des logs SSH | `auth.log` syslog OpenSSH | Format courant, proche des logs réels |
| Année absente des lignes syslog | 2026 | Les lignes syslog n'ont pas d'année ; hypothèse documentée |
| Fuseau | UTC en base (ISO 8601), fuseau indiqué dans l'interface | Exigence du cahier |
| Règle R1 | N = 5 échecs, T = 5 minutes, par IP | Valeurs pédagogiques, calibrées sur les scénarios de `ssh_signals.log` |
| Lignes invalides | Rejetées et comptées dans le bilan d'import ; l'import continue | Exigence du cahier (robustesse) |
| Données | Synthétiques pour la démonstration ; sources publiques en complément | Éviter les données sensibles |
| Licence | MIT | Même licence que l'exemple de référence ; permissive |

## Scénarios R1 retenus (`ssh_signals.log`)

| Scénario | IP | Échecs | Résultat attendu |
|---|---|---|---|
| S1 Force brute | 203.0.113.45 | 12 en environ 3 min | Alerte R1 |
| S2 Sous le seuil | 198.51.100.23 | 4 | Pas d'alerte |
| S3 Dépassement marqué puis succès | 203.0.113.99 | 30 en environ 4 min, puis connexion réussie | Alerte R1, priorité élevée |
| S4 Lent et discret | 192.0.2.60 | 6 sur 50 min | Pas d'alerte |
| S5 Limite | 198.51.100.77 | 5 en 4 min 40 s | Alerte R1 |

## Sources retenues

Liste complète avec liens et dates : [`docs/SOURCES.md`](docs/SOURCES.md).

- SSH : Lab6.2 (`auth.log`), Twente 2014 (COMIDDS), Loghub-2.0 (licence)
- HTTP : `elastic/examples` (apache_logs), `aagea/elk-example` (apache_logs)

## Décisions encore ouvertes

| Sujet | Statut |
|---|---|
| Application web locale ou desktop | À décider |
| Langage et framework | À décider |
| Seuils de R2 et R3 | À calibrer sur les logs HTTP |
| Politique de doublons et de réimport | À décider |
| Colonnes et encodage de l'export CSV | À décider |
| Critères exacts de gravité des événements | À décider |
