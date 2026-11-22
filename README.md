# MiniSOC

Application légère de supervision défensive **pour laboratoire** : elle importe des journaux SSH et HTTP depuis des fichiers, les normalise en événements communs, applique trois règles de corrélation (R1 répétitions SSH, R2 erreurs HTTP, R3 pic de volume), enregistre alertes et historique dans SQLite et les affiche dans un tableau de bord Streamlit exportable.

> Une alerte est un indice documenté, pas la preuve qu'une attaque a réussi.

## État d'avancement

| Élément | État |
|---|---|
| Structure du dépôt et publication sur GitHub | Fait |
| Décisions de cadrage | Fait (voir [`DECISIONS.md`](DECISIONS.md)) |
| Sources consultées | Fait (voir [`docs/SOURCES.md`](docs/SOURCES.md)) |
| Jeux de logs SSH synthétiques (référence + signaux) | À faire |
| Environnement Python et installation de Streamlit | Fait |
| Jeux de logs HTTP | À faire |
| Parsers, SQLite, règles, tableau de bord, export | À faire |

## Technologies

- Python (développé avec Python 3.15)
- Streamlit pour le tableau de bord
- SQLite (module `sqlite3` de Python) pour les événements et les alertes

## Prérequis et installation

Prérequis : Python 3.11 ou plus récent et `pip`. Sous Windows, utiliser la commande `py`.

```bat
git clone https://github.com/doomerC/minisoc.git
cd minisoc
py -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

## Lancement

À compléter une fois `app.py` écrit. La commande prévue est :

```bat
streamlit run app.py
```

## Architecture prévue

```
Fichiers SSH / HTTP -> validation et parsing -> événements normalisés -> SQLite -> moteur de règles -> alertes -> tableau de bord et export
```

## Structure du dépôt

```
minisoc/
├── app.py              tableau de bord Streamlit (à écrire)
├── db.py               SQLite : tables et initialisation (à écrire)
├── parsers.py          parsers SSH et HTTP (à écrire)
├── rules.py            règles R1, R2, R3 (à écrire)
├── requirements.txt    dépendances (streamlit)
├── README.md
├── DECISIONS.md        décisions prises
├── LICENSE
├── data/               jeux de logs synthétiques et scénarios (à ajouter)
├── docs/               RULES.md, SOURCES.md, SOURCES.pdf
├── exports/            rapports d'alertes exportés
├── screenshots/        captures du tableau de bord
└── tests/              tests (à ajouter)
```

## Jeux de logs

À faire : deux jeux synthétiques (référence et signaux) pour SSH et pour HTTP, avec la description de leurs scénarios.

## Règles

Détail et seuils : [`docs/RULES.md`](docs/RULES.md).

| Règle | Déclencheur | Seuils actuels |
|---|---|---|
| R1 – Répétitions SSH | au moins N échecs depuis une même IP en T minutes | N = 5, T = 5 min (valeurs pédagogiques) |
| R2 – Erreurs HTTP | au moins N réponses 4xx/5xx sur une fenêtre | à définir |
| R3 – Pic de volume | requêtes au-dessus d'un seuil sur une fenêtre | à définir |

## Décisions et sources

- Décisions : [`DECISIONS.md`](DECISIONS.md)
- Sources consultées : [`docs/SOURCES.md`](docs/SOURCES.md)

## Limites

Aucun agent intrusif, accès distant, blocage ou réponse automatique. Pas de collecte en production. Données synthétiques, seuils pédagogiques.

## Licence

MIT, voir [`LICENSE`](LICENSE).
