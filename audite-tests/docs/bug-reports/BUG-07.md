# BUG-07 : Le filtre « Livraison max » renvoie l'inverse du résultat attendu

| Champ | Valeur |
|---|---|
| Sévérité | **Majeure** |
| Priorité | **P2** |
| Statut | Ouvert |
| Cas de test | TC-07 |
| Zone | Page d'accueil, filtres |
| Environnement | QuickOrder V3.1 (commit d4606da), base réinitialisée par `seed.py`, Python 3.13, macOS, serveur local http://127.0.0.1:5055. |

## Contexte
Le client ne trouve pas les restaurants qui correspondent à son critère : perte de ventes.

## Préconditions
Frais de livraison : Green Kitchen 1,99 €, Pizza Roma 2,49 €, autres restaurants ≥ 2,99 €.

## Étapes de reproduction
1. Sur l'accueil, choisir « Livraison max ≤ 2,50 € ».
2. Cliquer « Rechercher ».

## Résultat attendu
Seuls Green Kitchen et Pizza Roma sont affichés.

## Résultat obtenu
Affichés : Couscous Atlas, Tokyo Bowl, Burger Factory, Night Sushi (tous ≥ 2,99 €). Les deux restaurants attendus sont absents.

## Preuve
- [TC-07-filtre-livraison.png](../evidence/png/TC-07-filtre-livraison.png)
- _À compléter par l'équipe si besoin : vidéo de la reproduction._
