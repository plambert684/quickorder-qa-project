# BUG-04 : Une quantité supérieure au stock est commandable et le stock devient négatif

| Champ | Valeur |
|---|---|
| Sévérité | **Majeure** |
| Priorité | **P1** |
| Statut | Ouvert |
| Cas de test | TC-11 |
| Zone | Panier, checkout, stock |
| Environnement | QuickOrder V3.1 (commit d4606da), base réinitialisée par `seed.py`, Python 3.13, macOS, serveur local http://127.0.0.1:5055. |

## Contexte
Survente, commandes impossibles à honorer, données de stock incohérentes.

## Préconditions
Alice connectée. Salmon Bowl (Tokyo Bowl) : stock = 2.

## Étapes de reproduction
1. Ajouter Salmon Bowl en quantité 3 au panier.
2. Passer commande par carte.
3. Consulter le stock (`GET /api/products/{id}`).

## Résultat attendu
L'ajout ou la commande est refusé (stock insuffisant) et le stock ne devient jamais négatif.

## Résultat obtenu
Commande #7 confirmée ; stock de Salmon Bowl après la commande : **-1**.

## Preuve
- [TC-11-1-ajout-quantite-3-stock-2.png](../evidence/png/TC-11-1-ajout-quantite-3-stock-2.png)
- [TC-11-2-commande.png](../evidence/png/TC-11-2-commande.png)
- [TC-11-3-stock-negatif.png](../evidence/png/TC-11-3-stock-negatif.png)
- [TC-11.txt](../evidence/TC-11.txt)
- _À compléter par l'équipe si besoin : vidéo de la reproduction._
