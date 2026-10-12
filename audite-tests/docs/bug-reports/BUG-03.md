# BUG-03 : Un restaurant fermé accepte des commandes

| Champ | Valeur |
|---|---|
| Sévérité | **Majeure** |
| Priorité | **P1** |
| Statut | Ouvert |
| Cas de test | TC-19 |
| Zone | Menu, panier, checkout |
| Environnement | QuickOrder V3.1 (commit d4606da), base réinitialisée par `seed.py`, Python 3.13, macOS, serveur local http://127.0.0.1:5055. |

## Contexte
Commandes payées qui ne pourront pas être préparées : remboursements et réclamations.

## Préconditions
Night Sushi est affiché « FERMÉ » (`is_open = false`).

## Étapes de reproduction
1. Ouvrir le menu Night Sushi.
2. Ajouter « Late Sushi Box » au panier.
3. Passer commande par carte.

## Résultat attendu
L'ajout ou la commande est refusé avec un message « restaurant fermé ».

## Résultat obtenu
Commande #9 créée au statut `confirmed`.

## Preuve
- [TC-19-1-ajout-restaurant-ferme.png](../evidence/png/TC-19-1-ajout-restaurant-ferme.png)
- [TC-19-2-commande.png](../evidence/png/TC-19-2-commande.png)
- _À compléter par l'équipe si besoin : vidéo de la reproduction._
