# BUG-01 : Un paiement PayPal crée deux commandes identiques (double débit)

| Champ | Valeur |
|---|---|
| Sévérité | **Critique** |
| Priorité | **P1** |
| Statut | Ouvert |
| Cas de test | TC-21 |
| Zone | Checkout, paiement PayPal |
| Environnement | QuickOrder V3.1 (commit d4606da), base réinitialisée par `seed.py`, Python 3.13, macOS, serveur local http://127.0.0.1:5055. |

## Contexte
Le client est débité deux fois et le restaurant reçoit deux commandes : perte financière directe, remboursements, perte de confiance.

## Préconditions
Alice connectée. Panier Pizza Roma : Margherita ×2 (24,29 € livraison incluse).

## Étapes de reproduction
1. Noter le nombre de commandes d'Alice (champ `count` de `GET /api/orders`).
2. Passer commande avec le mode de paiement **PayPal**.
3. Recompter les commandes.

## Résultat attendu
Une seule commande est créée et un seul paiement est enregistré.

## Résultat obtenu
Deux commandes identiques sont créées : avant = 10, après = 12. Les commandes #11 et #12 portent toutes deux `paypal`, 24,29 €.

## Preuve
- [TC-21-1-checkout-paypal.png](../evidence/png/TC-21-1-checkout-paypal.png)
- [TC-21-2-deux-commandes.png](../evidence/png/TC-21-2-deux-commandes.png)
- [TC-21.txt](../evidence/TC-21.txt)
- _À compléter par l'équipe si besoin : vidéo de la reproduction._
