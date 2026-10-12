# Stratégie de test : QuickOrder V3.1

## 1. Contexte

On audite QuickOrder avant sa mise en production. Objectif : trouver les bugs, les prouver, puis dire Go ou No-Go.

Version testée : V3.1, commit `d4606da`, données de `seed.py`.

## 2. Ce qu'on teste

| Domaine | Fonctionnalités |
|---|---|
| Compte | inscription, connexion, profil, adresses |
| Catalogue | restaurants, ouvert / fermé, recherche, filtres, tris |
| Panier | ajout, quantités, un seul restaurant, stock |
| Commande | checkout, coupons, adresse, paiement carte / PayPal / espèces, minimum de commande |
| Suivi | liste et détail des commandes, annulation |
| API REST | clé API, restaurants, produits, coupons, commandes |
| Accès | clé API obligatoire, commandes visibles seulement par leur client |

## 3. Ce qu'on ne teste pas

- Performance et charge (serveur de dev Flask).
- Vrai paiement (il est simulé).
- Autres navigateurs et mobile.
- Accessibilité.
- Sécurité poussée (CSRF, injections…), à faire dans un audit dédié.

## 4. Parcours critiques

1. Commander et payer.
2. Utiliser un coupon ou une promo.
3. Respecter les règles du restaurant : ouvert, stock, minimum.
4. Suivre et annuler sa commande.
5. Créer un compte puis commander.
6. Passer commande par l'API.

## 5. Risques principaux

Détail dans [risk-matrix.md](risk-matrix.md).

- Mauvais paiement (double débit, espèces non couvertes).
- Commande impossible à livrer (restaurant fermé, stock épuisé, adresse vide).
- Promo mal appliquée.
- Accès aux commandes d'un autre client.
- API qui plante.

## 6. Types de tests

| Type | Cas | Exemples | Outils |
|---|---|---|---|
| Nominal | 6 | connexion, panier, commande carte, annulation, commande API, espèces | navigateur, curl |
| Limite | 6 | filtre 2,50 €, coupon à 20,00 / 19,99 €, quantité 20 / 21, stock, minimum | navigateur, curl |
| Erreur | 6 | sans clé API, mauvais type, mauvais mot de passe, coupon expiré, commande d'un autre client, panier vide | navigateur, curl |
| Règle métier | 6 | restaurant fermé, espèces, PayPal, promo ATLAS15, un seul restaurant, adresse | navigateur, curl |
| Tests utilisateurs | 3 scénarios | 2 personnes minimum | voir [user-feedback.md](user-feedback.md) |
| Régression | 24 | on rejoue tout après chaque correction | cahier de test |

Interface : testée dans le navigateur, preuve par capture d'écran. API : testée avec curl, preuve par la réponse JSON. Tout est dans `docs/evidence/`.

## 7. Priorités

- Critique / Haute : argent, commande, accès aux données.
- Moyenne : le reste.

Ordre d'exécution : smoke tests, parcours nominaux, règles métier sur l'argent, limites, erreurs.

## 8. Critères d'entrée et de sortie

Pour commencer :

- l'appli démarre et `/api/health` répond `ok` ;
- `seed.py` passe sans erreur ;
- les tests `pytest` fournis passent.

Pour un Go :

- tous les cas exécutés ;
- aucun bug critique ou P1 ouvert ;
- 2 bugs majeurs maximum, avec contournement ;
- parcours critiques à 100 %.

## 9. Gestion des bugs

Chaque échec a son rapport dans `bug-reports/` : étapes, attendu, obtenu, preuve, sévérité, priorité. Après correction (dans une branche à part), on rejoue toute la campagne.

## 10. Livrables

- [Matrice des risques](risk-matrix.md)
- Cahier de test : [test-cases.xlsx](test-cases.xlsx) et [test-cases.md](test-cases.md)
- [Rapport d'exécution](execution-report.md)
- [Bug reports](bug-reports/README.md)
- [Tests utilisateurs](user-feedback.md)
- [Plan d'amélioration](improvement-plan.md)
