Décisions d'architecture et de cadrage — MiniSOC

Remarque de cadrage : MiniSOC est une application légère de supervision défensive en laboratoire visant à importer des journaux SSH et HTTP, normaliser les événements, exécuter des règles de corrélation transparentes, afficher un tableau de bord et persister l'historique et les alertes dans SQLite avec export CSV. Une alerte constitue un indice documenté pour prioriser les vérifications et non une conclusion qu'une attaque a réussi. Aucun agent intrusif, accès distant, blocage ou réponse automatique, collecte en production ni promesse d'exhaustivité n'est prévu.

1. Formats de logs et normalisation temporelle (UTC ISO 8601)
Journaux SSH (type syslog) : exemple observé Dec 10 06:55:46 ... — l'année et le fuseau horaire sont absents de la ligne.
Journaux HTTP (format combiné Apache/Nginx) : exemple observé [17/May/2015:10:05:03 +0000] — l'année et l'offset de fuseau (+0000) sont présents.
Standard retenu : conversion des horodatages en UTC ISO 8601 (YYYY-MM-DDTHH:MM:SSZ) pour homogénéiser le tri, le filtrage et les fenêtres de corrélation.
Hypothèses et honnêteté analytique : lorsqu'une information manque (ex. année ou fuseau absent dans les logs SSH), une hypothèse explicite est documentée (année supposée : 2026) et le fuseau affiché est indiqué clairement dans l'interface. Les données brutes utiles sont conservées pour permettre la vérification sans falsifier l'origine des logs.
2. Schéma conceptuel des données
Événement : événement normalisé issu d'une ligne de log (horodatage UTC ISO 8601, source ssh/http, entités clés comme IP/utilisateur/action ou méthode/statut/ressource, ainsi que la ligne/données brutes utiles).
Alerte : indice documenté produit par une règle de corrélation (gravité/priorité, règle déclenchée, justification/fenêtre temporelle, état de suivi), sans valeur de preuve définitive de compromission.
Import : trace d'un chargement de fichier (nom/source du fichier, date/heure d'import, nombre de lignes lues, normalisées, ignorées/invalides).
3. Gravité, lignes invalides, réimports et export CSV
Gravité / priorité : échelle lisible (ex. faible, moyenne, élevée, critique) complétée par "inconnue" lorsque la gravité ou un attribut n'est pas applicable ou non déterminé.
Lignes invalides / incomplètes : un échec de parsing sur une ligne n'interrompt pas l'import complet ; la ligne est comptabilisée comme ignorée et tracée pour inspection.
Réimports et doublons : le comportement face aux doublons/réimports est explicite et tracé afin de préserver la cohérence de l'historique et des alertes.
Export CSV : export disponible pour les alertes et l'historique des événements (UTF-8, colonnes explicites).
4. Technologies, structure du dépôt et règles
Langage et interface : Python avec Streamlit pour le tableau de bord (installé dans un environnement virtuel .venv).
Base de données : SQLite via le module sqlite3 de Python.
Hébergement : dépôt GitHub, partagé avec l'encadrant.
Structure du dépôt : à plat — app.py, db.py, parsers.py, rules.py, plus data/, docs/, exports/, screenshots/, tests/. Structure inspirée de l'exemple gaston-dev-web/mini-siem (aucun code copié).
Règle R1 (répétitions SSH) : valeurs de départ N = 5 échecs en T = 5 minutes par IP ; valeurs pédagogiques, à calibrer sur les jeux de logs.
Licence : MIT.
5. Sources et références consultées
Documentation officielle Python (datetime, sqlite3, csv) — consultée le 2026-11-21 — https://docs.python.org/3/
Documentation officielle Streamlit — consultée le 2026-11-21 — https://docs.streamlit.io/
Documentation officielle SQLite — consultée le 2026-11-21 — https://www.sqlite.org/docs.html
Logs SSH : Lab6.2 auth.log — https://github.com/munetworkforensics/Lab6.2/blob/master/auth.log ; Twente 2014 (COMIDDS) — https://fkie-cad.github.io/COMIDDS/content/datasets/twente_2014/ ; licence Loghub-2.0 — https://github.com/logpai/loghub-2.0/blob/main/LICENSE
Logs HTTP : elastic/examples apache_logs — https://github.com/elastic/examples/blob/master/Common%20Data%20Formats/apache_logs/apache_logs ; aagea/elk-example apache_logs — https://github.com/aagea/elk-example/blob/master/apache_logs
Liste détaillée avec dates : docs/SOURCES.md
6. Décisions encore ouvertes
Jeux de logs synthétiques (référence et signaux) et description de leurs scénarios : à faire.
Seuils de R2 et R3 : à calibrer sur les logs HTTP.
Politique précise de doublons et de réimport : à préciser.
Colonnes exactes de l'export CSV : à fixer.
Critères exacts de gravité : à fixer.
