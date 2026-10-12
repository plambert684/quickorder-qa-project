# Matrice des risques : QuickOrder V3.1

Note de 1 à 4 pour la probabilité et l'impact. Criticité = probabilité × impact. Dernière colonne : ce que les tests ont montré.

Impact : 1 gêne, 2 fonction dégradée, 3 commande non honorée, 4 perte d'argent / problème juridique / sécurité.

Probabilité : 1 rare, 2 possible, 3 probable, 4 quasi certain.

| ID | Risque | Proba | Impact | Criticité | Cas de test | Résultat des tests |
|---|---|---|---|---|---|---|
| R1 | Double débit au paiement | 3 | 4 | **12** | TC-03, TC-21 | Bug avec PayPal (BUG-01), la carte marche |
| R2 | Espèces acceptées sans être couvertes | 3 | 4 | **12** | TC-06, TC-20 | Bug (BUG-02), un montant suffisant marche |
| R3 | Commande dans un restaurant fermé | 3 | 3 | **9** | TC-19 | Bug (BUG-03) |
| R4 | Vente au-delà du stock | 3 | 3 | **9** | TC-11 | Bug (BUG-04) |
| R5 | Commande sans adresse | 3 | 3 | **9** | TC-24 | Bug (BUG-05) |
| R6 | Promo affichée ≠ promo appliquée | 3 | 3 | **9** | TC-08, TC-09, TC-16, TC-22 | Bug sur ATLAS15 (BUG-08), WELCOME10 et EXPIRED10 OK |
| R7 | Accès aux commandes d'un autre client / API sans clé | 2 | 4 | **8** | TC-13, TC-17 | Pas de bug |
| R8 | Totaux du panier faux | 2 | 4 | **8** | TC-02, TC-03, TC-05 | Pas de bug |
| R9 | Minimum de commande ignoré | 3 | 2 | **6** | TC-12 | Bug (BUG-06) |
| R10 | Filtre qui renvoie de mauvais résultats | 3 | 2 | **6** | TC-07 | Bug (BUG-07) |
| R11 | Panier incohérent | 2 | 2 | **4** | TC-18, TC-23 | Pas de bug |
| R12 | API qui plante | 2 | 2 | **4** | TC-05, TC-10, TC-14 | Bug : erreur 500 (BUG-09), limites de quantité OK |
| R13 | Connexion défaillante | 1 | 4 | **4** | TC-01, TC-15 | Pas de bug |
| R14 | Annulation impossible | 1 | 3 | **3** | TC-04 | Pas de bug |

## En résumé

- Les 6 risques les plus élevés (criticité ≥ 9) ont tous donné un bug.
- Calculs, connexion et accès aux données : rien à signaler.
- Le problème, ce sont les règles métier : affichées, mais pas appliquées.
