# Tests utilisateurs : QuickOrder V3.1

> À faire par l'équipe avec au moins 2 vraies personnes. On remplit les grilles pendant les sessions, sans rien inventer.

## 1. Déroulé

- Participants : 2 personnes hors projet, si possible une habituée des applis de livraison et une moins à l'aise.
- Durée : 20 minutes environ par personne.
- La personne réfléchit à voix haute. On ne l'aide pas, on note.
- Matériel : QuickOrder en local (`python seed.py` puis `python run.py`), un navigateur, un chrono.
- On donne l'objectif, jamais le chemin.

## 2. Scénarios

Choisis pour passer là où on a trouvé des bugs.

| # | Consigne | On observe | Bugs liés |
|---|---|---|---|
| S1 | « Trouvez un restaurant avec une livraison à 2,50 € maximum et commandez un plat végétarien. » | Le filtre « Livraison max » est-il compris ? Fait-on confiance au résultat ? | BUG-07, BUG-04 |
| S2 | « Créez un compte et commandez chez Couscous Atlas avec la promo affichée. Payez en espèces. » | Saisie de l'adresse, remise vraiment obtenue, montant remis | BUG-05, BUG-08, BUG-02 |
| S3 | « Corrigez une quantité dans votre panier, puis annulez la commande passée. » | Modification du panier, bouton d'annulation, statuts | BUG-01 (si PayPal) |
| S4 (optionnel) | « Trouvez un restaurant qui fait des salades et laissez un avis. » | Recherche par plat, avis visible ou non | aucun |

## 3. Participants

| | Participant 1 | Participant 2 |
|---|---|---|
| Profil (âge, métier, aisance numérique 1-5) | | |
| Utilise des applis de livraison ? (jamais / parfois / souvent) | | |
| Appareil et navigateur | | |
| Date et durée | | |

## 4. Problèmes observés

| ID | Scénario | Participant | Où / quand | Problème | Fréquence (x/2) | Impact (bloquant / gênant / mineur) | Ce qu'a dit la personne | Idée d'amélioration | Priorité |
|---|---|---|---|---|---|---|---|---|---|
| UF-01 | | | | | | | | | |
| UF-02 | | | | | | | | | |
| UF-03 | | | | | | | | | |
| UF-04 | | | | | | | | | |
| UF-05 | | | | | | | | | |
| UF-06 | | | | | | | | | |

## 5. Résultats par scénario

| Scénario | Participant | Réussi seul (O/N) | Temps | Erreurs | Satisfaction (1-5) |
|---|---|---|---|---|---|
| S1 | P1 | | | | |
| S1 | P2 | | | | |
| S2 | P1 | | | | |
| S2 | P2 | | | | |
| S3 | P1 | | | | |
| S3 | P2 | | | | |

## 6. Bilan (après les sessions)

- Ce qui revient le plus :
- Ce qui bloque le plus :
- Bugs de la campagne retrouvés :
- Nouveaux problèmes :
- Ajouts au [plan d'amélioration](improvement-plan.md) :
