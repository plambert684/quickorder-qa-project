# Bug reports

| ID | Titre | Sévérité | Priorité | Cas de test |
|---|---|---|---|---|
| [BUG-01](BUG-01.md) | Un paiement PayPal crée deux commandes identiques (double débit) | Critique | P1 | TC-21 |
| [BUG-02](BUG-02.md) | Paiement en espèces accepté avec un montant inférieur au total | Majeure | P1 | TC-20 |
| [BUG-03](BUG-03.md) | Un restaurant fermé accepte des commandes | Majeure | P1 | TC-19 |
| [BUG-04](BUG-04.md) | Une quantité supérieure au stock est commandable et le stock devient négatif | Majeure | P1 | TC-11 |
| [BUG-05](BUG-05.md) | Un nouveau compte peut commander avec une adresse de livraison vide | Majeure | P1 | TC-24 |
| [BUG-06](BUG-06.md) | Le minimum de commande affiché n'est pas appliqué | Majeure | P2 | TC-12 |
| [BUG-07](BUG-07.md) | Le filtre « Livraison max » renvoie l'inverse du résultat attendu | Majeure | P2 | TC-07 |
| [BUG-08](BUG-08.md) | Le code ATLAS15 annoncé à -15 % n'applique que 10 % | Majeure | P2 | TC-22 |
| [BUG-09](BUG-09.md) | Un `product_id` envoyé en texte à l'API provoque une erreur 500 | Moyenne | P2 | TC-14 |

**Sévérité** : Critique > Majeure > Moyenne > Mineure.

**Priorité** :

- P1 : bloquant pour la mise en production ;
- P2 : à corriger avant l'ouverture large ;
- P3 : backlog.
