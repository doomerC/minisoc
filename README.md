# MiniSOC

Application légère de supervision défensive **pour laboratoire** : elle importe des journaux SSH et HTTP depuis des fichiers, les normalise en événements communs, applique trois règles de corrélation (R1 répétitions SSH, R2 erreurs HTTP, R3 pic de volume), enregistre alertes et historique dans SQLite et les affiche dans un tableau de bord exportable.

> Une alerte est un indice documenté, pas la preuve qu'une attaque a réussi.

## État d'avancement

| Élément | État |
|---|---|
| Structure du dépôt | Fait |
| Décisions de cadrage | Fait (voir [`DECISIONS.md`](DECISIONS.md)) |
| Sources consultées | Fait (voir [`docs/SOURCES.md`](docs/SOURCES.md)) |
| Jeux de logs SSH synthétiques (référence + signaux) | Fait |
| Jeux de logs HTTP | À faire |
| Parsers, SQLite, règles, tableau de bord, export | À faire |

## Architecture prévue

```
Fichiers SSH / HTTP -> validation et parsing -> événements normalisés -> SQLite -> moteur de règles -> alertes -> tableau de bord et export
```

## Structure du dépôt

```
minisoc/
├── app.py              application web et tableau de bord (à écrire)
├── db.py               SQLite : tables et initialisation (à écrire)
├── parsers.py          parsers SSH et HTTP (à écrire)
├── rules.py            règles R1, R2, R3 (à écrire)
├── requirements.txt
├── README.md
├── DECISIONS.md        décisions prises
├── LICENSE
├── data/               jeux de logs synthétiques et scénarios
├── docs/               RULES.md, SOURCES.md, SOURCES.pdf
├── exports/            rapports d'alertes exportés
├── screenshots/        captures du tableau de bord
└── tests/              check_r1.py et tests
```

## Jeux de logs

| Fichier | Contenu |
|---|---|
| `data/ssh_reference.log` | SSH synthétique, activité normale : aucune alerte R1 attendue |
| `data/ssh_signals.log` | SSH synthétique avec 5 scénarios (S1 à S5) et des lignes invalides |
| `data/SCENARIOS_SSH.md` | Description des scénarios et résultats attendus |

Les données sont synthétiques (adresses IP de documentation RFC 5737). Le script `tests/check_r1.py` recalcule R1 de façon indépendante pour comparer avec le moteur de règles.

## Règles

Détail et seuils : [`docs/RULES.md`](docs/RULES.md).

| Règle | Déclencheur | Seuils actuels |
|---|---|---|
| R1 – Répétitions SSH | au moins N échecs depuis une même IP en T minutes | N = 5, T = 5 min (valeurs pédagogiques) |
| R2 – Erreurs HTTP | au moins N réponses 4xx/5xx sur une fenêtre | à définir |
| R3 – Pic de volume | requêtes au-dessus d'un seuil sur une fenêtre | à définir |

## Prérequis, installation, lancement, import

À compléter une fois l'application écrite.

## Décisions et sources

- Décisions : [`DECISIONS.md`](DECISIONS.md)
- Sources consultées : [`docs/SOURCES.md`](docs/SOURCES.md)

## Limites

Aucun agent intrusif, accès distant, blocage ou réponse automatique. Pas de collecte en production. Données synthétiques, seuils pédagogiques.

## Licence

MIT, voir [`LICENSE`](LICENSE).
