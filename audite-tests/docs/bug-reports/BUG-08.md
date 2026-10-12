# BUG-08 : Le code ATLAS15 annoncé à -15 % n'applique que 10 %

| Champ | Valeur |
|---|---|
| Sévérité | **Majeure** |
| Priorité | **P2** |
| Statut | Ouvert |
| Cas de test | TC-22 |
| Zone | Coupons, Couscous Atlas |
| Environnement | QuickOrder V3.1 (commit d4606da), base réinitialisée par `seed.py`, Python 3.13, macOS, serveur local http://127.0.0.1:5055. |

## Contexte
Promotion trompeuse : risque juridique (pratique commerciale trompeuse) et réclamations clients.

## Préconditions
La page Couscous Atlas affiche « utilisez le code ATLAS15 pour obtenir 15 % de réduction ».

## Étapes de reproduction
1. Ajouter Couscous Royal ×2 (35,80 €).
2. Au checkout, saisir ATLAS15 et cliquer « Appliquer ».

## Résultat attendu
Réduction de 15 %, soit 5,37 €.

## Résultat obtenu
Réduction appliquée : 3,58 €, soit 10 %.

## Preuve
- [TC-22-1-offre-15-pourcent.png](../evidence/png/TC-22-1-offre-15-pourcent.png)
- [TC-22-2-reduction-appliquee.png](../evidence/png/TC-22-2-reduction-appliquee.png)
- _À compléter par l'équipe si besoin : vidéo de la reproduction._
