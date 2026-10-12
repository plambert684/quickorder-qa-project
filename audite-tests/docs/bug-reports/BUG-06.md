# BUG-06 : Le minimum de commande affiché n'est pas appliqué

| Champ | Valeur |
|---|---|
| Sévérité | **Majeure** |
| Priorité | **P2** |
| Statut | Ouvert |
| Cas de test | TC-12 |
| Zone | Checkout |
| Environnement | QuickOrder V3.1 (commit d4606da), base réinitialisée par `seed.py`, Python 3.13, macOS, serveur local http://127.0.0.1:5055. |

## Contexte
La règle commerciale du restaurant n'est pas respectée : commandes non rentables.

## Préconditions
Burger Factory : « Minimum 20.00 € » affiché sur la liste et au checkout.

## Étapes de reproduction
1. Ajouter Classic Burger ×1 et Frites maison ×1 (sous-total 15,40 €).
2. Ouvrir le checkout et confirmer par carte.

## Résultat attendu
Commande refusée avec un message « Minimum de commande de 20,00 € non atteint ».

## Résultat obtenu
Commande #8 confirmée, total 18,39 € (15,40 € + 2,99 € de livraison).

## Preuve
- [TC-12-1-checkout-minimum.png](../evidence/png/TC-12-1-checkout-minimum.png)
- [TC-12-2-commande.png](../evidence/png/TC-12-2-commande.png)
- _À compléter par l'équipe si besoin : vidéo de la reproduction._
