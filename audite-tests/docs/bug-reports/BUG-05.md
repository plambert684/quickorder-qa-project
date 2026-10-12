# BUG-05 : Un nouveau compte peut commander avec une adresse de livraison vide

| Champ | Valeur |
|---|---|
| Sévérité | **Majeure** |
| Priorité | **P1** |
| Statut | Ouvert |
| Cas de test | TC-24 |
| Zone | Inscription et checkout |
| Environnement | QuickOrder V3.1 (commit d4606da), base réinitialisée par `seed.py`, Python 3.13, macOS, serveur local http://127.0.0.1:5055. |

## Contexte
Commande payée impossible à livrer, dès la première commande d'un nouveau client.

## Préconditions
Compte créé via /register, sans saisir d'adresse.

## Étapes de reproduction
1. Créer un compte.
2. Ajouter Margherita ×2 au panier.
3. Ouvrir le checkout : l'adresse proposée est « Domicile — ,   ».
4. Confirmer par carte.

## Résultat attendu
Le checkout exige une adresse complète (rue, code postal, ville) avant la confirmation.

## Résultat obtenu
L'inscription crée automatiquement une adresse « Domicile » vide, et la commande #13 est confirmée avec une adresse de livraison vide.

## Preuve
- [TC-24-1-checkout-adresse-vide.png](../evidence/png/TC-24-1-checkout-adresse-vide.png)
- [TC-24-2-commande-sans-adresse.png](../evidence/png/TC-24-2-commande-sans-adresse.png)
- _À compléter par l'équipe si besoin : vidéo de la reproduction._
