# BUG-02 : Paiement en espèces accepté avec un montant inférieur au total

| Champ | Valeur |
|---|---|
| Sévérité | **Majeure** |
| Priorité | **P1** |
| Statut | Ouvert |
| Cas de test | TC-20 |
| Zone | Checkout, paiement en espèces |
| Environnement | QuickOrder V3.1 (commit d4606da), base réinitialisée par `seed.py`, Python 3.13, macOS, serveur local http://127.0.0.1:5055. |

## Contexte
La commande est considérée comme payée alors qu'elle ne l'est pas, et le livreur devrait rendre une monnaie négative.

## Préconditions
Alice connectée. Panier Pizza Roma : Margherita ×2, total 24,29 €.

## Étapes de reproduction
1. Au checkout, choisir **Espèces**.
2. Saisir `1` dans « Montant remis en espèces ».
3. Confirmer.

## Résultat attendu
Commande refusée : le montant remis doit être supérieur ou égal au total (24,29 €).

## Résultat obtenu
Commande #10 créée au statut `confirmed`, paiement `paid`, « Monnaie calculée : -23.29 € ».

## Preuve
- [TC-20-1-checkout-especes-1-euro.png](../evidence/png/TC-20-1-checkout-especes-1-euro.png)
- [TC-20-2-commande.png](../evidence/png/TC-20-2-commande.png)
- _À compléter par l'équipe si besoin : vidéo de la reproduction._
