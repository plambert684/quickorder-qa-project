# Plan d'amélioration : QuickOrder V3.1

Classé par gravité du bug et effort (S : moins d'1 jour, M : 1 à 3 jours). À compléter après les tests utilisateurs.

| # | Amélioration | Bugs | Priorité | Effort | Pourquoi |
|---|---|---|---|---|---|
| A1 | Une seule commande par paiement ; refuser les espèces inférieures au total ; `paid` seulement si c'est payé | BUG-01, BUG-02 | P1 | S | Perte d'argent directe. |
| A2 | Un seul contrôle de commande, commun à l'interface et à l'API : restaurant ouvert, stock, minimum | BUG-03, BUG-04, BUG-06 | P1 | M | Ces règles sont affichées mais jamais vérifiées. Un seul endroit à corriger. |
| A3 | Adresse complète obligatoire avant la première commande, plus d'adresse vide à l'inscription | BUG-05 | P1 | S | Sinon la commande est payée mais pas livrable. |
| A4 | Construire le texte de la promo à partir du coupon enregistré | BUG-08 | P2 | S | Fini les promos annoncées à -15 % qui donnent -10 %. |
| A5 | Corriger le filtre de livraison (`<=`) et tester 2,49 / 2,50 / 2,51 € | BUG-07 | P2 | S | Le client ne trouve pas les restaurants dans son budget. |
| A6 | Valider les types côté API et renvoyer des erreurs en JSON | BUG-09 | P2 | M | Pas d'erreur 500 sur une simple faute de saisie. Corrige aussi le test `pytest` en échec. |
| A7 | Bouton « Ajouter » désactivé quand le produit est épuisé | BUG-04 | P3 | S | Le client le voit tout de suite. |
| A8 | Automatiser les 24 cas et `pytest` dans une CI à chaque pull request | Tous | P2 | M | Évite de recasser ce qui a été corrigé. |

## Ordre proposé

1. Sprint 1 (bloquant) : A1, A2, A3, puis on rejout les 24 cas.
2. Sprint 2 : A4, A5, A6, A8.
3. Ensuite : A7 et les retours des tests utilisateurs.

Les corrections se font dans une branche à part, puis on revalide chaque bug.
