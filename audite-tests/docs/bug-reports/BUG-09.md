# BUG-09 : Un `product_id` envoyé en texte à l'API provoque une erreur 500

| Champ | Valeur |
|---|---|
| Sévérité | **Moyenne** |
| Priorité | **P2** |
| Statut | Ouvert |
| Cas de test | TC-14 |
| Zone | API POST /api/orders |
| Environnement | QuickOrder V3.1 (commit d4606da), base réinitialisée par `seed.py`, Python 3.13, macOS, serveur local http://127.0.0.1:5055. |

## Contexte
Robustesse insuffisante : une requête mal typée d'un client API fait planter le serveur.

## Préconditions
Clé API valide.

## Étapes de reproduction
1. Envoyer `POST /api/orders` avec `{"items": [{"product_id": "16", "quantity": 1}]}`.

## Résultat attendu
HTTP 400 (validation) ou 201 (conversion), avec un corps JSON.

## Résultat obtenu
HTTP **500** Internal Server Error, avec une page HTML.

## Preuve
- [TC-14.txt](../evidence/TC-14.txt)
- _À compléter par l'équipe si besoin : vidéo de la reproduction._
