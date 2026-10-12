# Rapport d'exécution : QuickOrder V3.1

| | |
|---|---|
| Date | 12/10/2026 |
| Version | V3.1, commit `d4606da` |
| Environnement | macOS, Python 3.13.2, Flask en local, base remise à zéro par `seed.py` |
| Outils | navigateur (interface), curl (API), Swagger UI |
| Données | comptes `alice`, `bob`, `admin`, 6 restaurants, 6 coupons |
| Preuves | `docs/evidence/` : captures d'écran (`png/`) et réponses JSON de l'API (index dans `evidence/README.md`) |

## 0. Avant de commencer

| Vérification | OK ? |
|---|---|
| L'appli démarre, `/api/health` répond `ok` | Oui |
| `seed.py` passe sans erreur | Oui |
| Tests `pytest` fournis | Non : 5 sur 6. `test_api_orders_with_key` plante (`AttributeError` dans `app/routes/api.py`) |

Pourquoi : l'API utilise toujours le compte `alice@example.com`, absent de la base de `pytest`. Ça ne gêne pas la campagne, qui tourne sur les données de démo.

## 1. Résultats

| Indicateur | Valeur |
|---|---|
| Cas de test | **24** |
| Pass | **15** (62,5 %) |
| Fail | **9** (37,5 %) |
| Blocked | 0 |
| Skipped | 0 |
| Bugs | **9** (1 critique, 7 majeurs, 1 moyen) |

### Par type

| Type | Exécutés | Pass | Fail | Réussite |
|---|---|---|---|---|
| Nominal | 6 | 6 | 0 | 100 % |
| Limite | 6 | 3 | 3 | 50 % |
| Erreur | 6 | 5 | 1 | 83 % |
| Règle métier | 6 | 1 | 5 | **17 %** |

### Par priorité

| Priorité | Exécutés | Fail |
|---|---|---|
| Critique | 1 | 1 |
| Haute | 18 | 7 |
| Moyenne | 5 | 1 |

### Les 24 cas

| ID | Type | Cas | Statut | Bug |
|---|---|---|---|---|
| TC-01 | Nominal | Connexion avec identifiants valides | Pass | |
| TC-02 | Nominal | Ajout au panier et calcul des totaux | Pass | |
| TC-03 | Nominal | Commande par carte avec coupon WELCOME10 | Pass | |
| TC-04 | Nominal | Annulation d'une commande confirmée | Pass | |
| TC-05 | Nominal | API : création d'une commande par carte | Pass | |
| TC-06 | Nominal | Paiement en espèces avec montant suffisant | Pass | |
| TC-07 | Limite | Filtre « Livraison max ≤ 2,50 € » | Fail | BUG-07 |
| TC-08 | Limite | Coupon au minimum exact (20,00 €) | Pass | |
| TC-09 | Limite | Coupon juste sous le minimum (19,99 €) | Pass | |
| TC-10 | Limite | API : quantité 20 acceptée, 21 refusée | Pass | |
| TC-11 | Limite | Quantité supérieure au stock restant | Fail | BUG-04 |
| TC-12 | Limite | Commande sous le minimum du restaurant | Fail | BUG-06 |
| TC-13 | Erreur | API : clé absente | Pass | |
| TC-14 | Erreur | API : product_id envoyé sous forme de texte | Fail | BUG-09 |
| TC-15 | Erreur | Connexion avec mauvais mot de passe | Pass | |
| TC-16 | Erreur | Coupon expiré EXPIRED10 | Pass | |
| TC-17 | Erreur | Accès à la commande d'un autre client | Pass | |
| TC-18 | Erreur | Checkout avec panier vide | Pass | |
| TC-19 | Règle métier | Restaurant fermé non commandable | Fail | BUG-03 |
| TC-20 | Règle métier | Espèces : montant insuffisant | Fail | BUG-02 |
| TC-21 | Règle métier | PayPal : une seule commande créée | Fail | BUG-01 |
| TC-22 | Règle métier | Promotion ATLAS15 annoncée à -15 % | Fail | BUG-08 |
| TC-23 | Règle métier | Panier limité à un seul restaurant | Pass | |
| TC-24 | Règle métier | Adresse exigée pour un nouveau compte | Fail | BUG-05 |

## 2. Ce qu'on en retient

- Les parcours normaux marchent : 6 sur 6.
- Les erreurs sont bien gérées : 5 sur 6. Seule exception, une erreur 500 sur l'API.
- Les règles métier ne sont pas appliquées : 5 sur 6 en échec. « FERMÉ », « Minimum 20 € », « -15 % » sont affichés mais pas respectés.
- Le paiement posent un problème : PayPal crée deux commandes, et 1 € en espèces suffit pour payer 24,29 €.

## 3. Bugs par sévérité

| Sévérité | Bugs |
|---|---|
| Critique | BUG-01 PayPal en double |
| Majeure | BUG-02 espèces, BUG-03 restaurant fermé, BUG-04 stock négatif, BUG-05 adresse vide, BUG-06 minimum ignoré, BUG-07 filtre inversé, BUG-08 ATLAS15 à 10 % |
| Moyenne | BUG-09 erreur 500 API |

Détail : [bug-reports/README.md](bug-reports/README.md).

## 4. Risques si on livre en l'état

- Argent : double débit PayPal, espèces non payées, promo fausse.
- Livraison : commandes impossibles à honorer (restaurant fermé, stock, adresse vide).
- Juridique : promo « -15 % » non tenue.

## 5. Recommandation : NO-GO

Un bug critique et cinq bugs P1 sont ouverts. Le paiement échoue avec 2 moyens sur 3.

Pour passer en Go :

- corrigé BUG-01 à BUG-05 (P1), puis BUG-06 à BUG-09 (P2) ;
- rejouer les 24 cas avec 100 % de réussite ;
- revalider chaque bug.

## 6. Limites

- Un seul navigateur, pas de mobile.
- Tests utilisateurs pas encore faits (voir [user-feedback.md](user-feedback.md)).
