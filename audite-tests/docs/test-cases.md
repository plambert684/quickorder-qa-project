# Cas de test : QuickOrder V3.1

Exécutés le 12/10/2026 en local. Preuves dans `evidence/`. Version Excel : `test-cases.xlsx`.

| Type | Nombre | Pass | Fail |
|---|---|---|---|
| Nominal | 6 | 6 | 0 |
| Limite | 6 | 3 | 3 |
| Erreur | 6 | 5 | 1 |
| Règle métier | 6 | 1 | 5 |

## TC-01 : Connexion d'un client avec identifiants valides

Type : Nominal, priorité : Haute, statut : **Pass**

- **Objectif** : Vérifier l'authentification d'un client existant.
- **Préconditions** : Compte alice@example.com existant.
- **Données** : alice@example.com / Password123!
- **Étapes** :
  1. Ouvrir /login
  2. Saisir email et mot de passe
  3. Valider
- **Résultat attendu** : Redirection vers l'accueil ; le nom « Alice Martin » et le lien « Déconnexion » apparaissent.
- **Résultat obtenu** : Redirection vers l'accueil ; « Alice Martin » et « Déconnexion » s'affichent.
- **Preuve** : [TC-01-1-formulaire.png](evidence/png/TC-01-1-formulaire.png), [TC-01-2-connecte.png](evidence/png/TC-01-2-connecte.png)

## TC-02 : Ajout de produits au panier et calcul des totaux

Type : Nominal, priorité : Haute, statut : **Pass**

- **Objectif** : Vérifier l'ajout au panier et le calcul sous-total / livraison / total.
- **Préconditions** : Client connecté, panier vide.
- **Données** : Burger Factory : Classic Burger ×2 (11,90 €), Frites maison ×1 (3,50 €) ; livraison 2,99 €
- **Étapes** :
  1. Ouvrir le menu Burger Factory
  2. Ajouter Classic Burger qté 2
  3. Ajouter Frites qté 1
  4. Ouvrir /cart
- **Résultat attendu** : Sous-total 27,30 € ; livraison 2,99 € ; total 30,29 €.
- **Résultat obtenu** : Panier : sous-total 27,30 €, livraison 2,99 €, total 30,29 €.
- **Preuve** : [TC-02-panier.png](evidence/png/TC-02-panier.png)

## TC-03 : Commande par carte avec coupon WELCOME10

Type : Nominal, priorité : Haute, statut : **Pass**

- **Objectif** : Vérifier le parcours de commande complet par carte avec un coupon valide.
- **Préconditions** : Client connecté avec adresse ; panier Burger Factory de 27,30 €.
- **Données** : Coupon WELCOME10 (-10 %, min 20 €, plafond 8 €) ; paiement carte
- **Étapes** :
  1. Remplir le panier (TC-02)
  2. Ouvrir /checkout
  3. Saisir WELCOME10
  4. Choisir Carte
  5. Confirmer
- **Résultat attendu** : Commande créée au statut confirmed, paiement paid ; réduction 2,73 € ; total 27,56 €.
- **Résultat obtenu** : Commande #2 créée : statut confirmed, paiement paid / card, réduction 2,73 €, total 27,56 €.
- **Preuve** : [TC-03-1-checkout.png](evidence/png/TC-03-1-checkout.png), [TC-03-2-commande.png](evidence/png/TC-03-2-commande.png)

## TC-04 : Annulation d'une commande confirmée

Type : Nominal, priorité : Haute, statut : **Pass**

- **Objectif** : Vérifier qu'un client peut annuler sa commande tant qu'elle est confirmée.
- **Préconditions** : Client connecté ; une commande au statut confirmed.
- **Données** : Commande créée en préparation du test
- **Étapes** :
  1. Passer une commande
  2. Ouvrir son détail
  3. Cliquer « Annuler la commande »
- **Résultat attendu** : Message « Commande annulée. » ; statut cancelled.
- **Résultat obtenu** : Message « Commande annulée. » ; la commande #3 passe au statut cancelled.
- **Preuve** : [TC-04-annulation.png](evidence/png/TC-04-annulation.png)

## TC-05 : Création d'une commande par carte via l'API

Type : Nominal, priorité : Haute, statut : **Pass**

- **Objectif** : Vérifier POST /api/orders sur un cas nominal.
- **Préconditions** : Clé API valide ; compte démo alice avec adresse.
- **Données** : Margherita ×2 (10,90 €) chez Pizza Roma (livraison 2,49 €, minimum 12 €) ; card
- **Étapes** :
  1. POST /orders avec le JSON de données
- **Résultat attendu** : HTTP 201 ; status confirmed ; subtotal 21,80 ; total 24,29.
- **Résultat obtenu** : HTTP 201 ; commande #4 : status confirmed, subtotal 21,80, total 24,29.
- **Preuve** : [TC-05.txt](evidence/TC-05.txt)

## TC-06 : Paiement en espèces avec montant suffisant

Type : Nominal, priorité : Moyenne, statut : **Pass**

- **Objectif** : Vérifier le calcul de la monnaie rendue.
- **Préconditions** : Client connecté.
- **Données** : Pizza Roma : Margherita ×2 = 21,80 € + 2,49 € = 24,29 € ; espèces remises 30,00 €
- **Étapes** :
  1. Remplir le panier
  2. Checkout, choisir Espèces, saisir 30
  3. Confirmer
- **Résultat attendu** : Commande confirmée ; monnaie calculée 5,71 €.
- **Résultat obtenu** : Commande #5 confirmée ; « Monnaie calculée : 5.71 € ».
- **Preuve** : [TC-06-1-checkout-especes.png](evidence/png/TC-06-1-checkout-especes.png), [TC-06-2-commande.png](evidence/png/TC-06-2-commande.png)

## TC-07 : Filtre « Livraison max ≤ 2,50 € »

Type : Limite, priorité : Haute, statut : **Fail** (**BUG-07**)

- **Objectif** : Vérifier le filtre sur le frais de livraison à la valeur limite.
- **Préconditions** : -
- **Données** : max_delivery = 2.50 ; frais : Green Kitchen 1,99 / Pizza Roma 2,49 / autres ≥ 2,99
- **Étapes** :
  1. Choisir « Livraison max ≤ 2,50 € »
  2. Rechercher
- **Résultat attendu** : Seuls les restaurants dont la livraison est ≤ 2,50 € : Green Kitchen et Pizza Roma.
- **Résultat obtenu** : Restaurants affichés : Couscous Atlas, Tokyo Bowl, Burger Factory, Night Sushi (livraison 2,99 € à 3,49 €). Green Kitchen et Pizza Roma n'apparaissent pas.
- **Preuve** : [TC-07-filtre-livraison.png](evidence/png/TC-07-filtre-livraison.png)

## TC-08 : Coupon WELCOME10 avec un sous-total de 20,00 € (minimum exact)

Type : Limite, priorité : Haute, statut : **Pass**

- **Objectif** : Vérifier la borne inclusive du montant minimum du coupon.
- **Préconditions** : Clé API valide.
- **Données** : code WELCOME10 ; subtotal 20.00
- **Étapes** :
  1. POST /coupons/validate
- **Résultat attendu** : valid = true ; discount = 2,00.
- **Résultat obtenu** : HTTP 200 ; valid = true, discount = 2.0.
- **Preuve** : [TC-08.txt](evidence/TC-08.txt)

## TC-09 : Coupon WELCOME10 avec un sous-total de 19,99 € (sous le minimum)

Type : Limite, priorité : Haute, statut : **Pass**

- **Objectif** : Vérifier la borne basse du coupon.
- **Préconditions** : Clé API valide.
- **Données** : code WELCOME10 ; subtotal 19.99
- **Étapes** :
  1. POST /coupons/validate
- **Résultat attendu** : valid = false ; message « Montant minimum non atteint. ».
- **Résultat obtenu** : HTTP 200 ; valid = false, « Montant minimum non atteint. », minimum = 20.0.
- **Preuve** : [TC-09.txt](evidence/TC-09.txt)

## TC-10 : Quantité 20 acceptée et 21 refusée par l'API

Type : Limite, priorité : Moyenne, statut : **Pass**

- **Objectif** : Vérifier la borne haute documentée (maximum 20).
- **Préconditions** : Clé API valide ; Tiramisu stock 100.
- **Données** : Tiramisu quantity = 20 puis 21
- **Étapes** :
  1. POST /orders qté 20
  2. POST /orders qté 21
- **Résultat attendu** : qté 20 : HTTP 201 ; qté 21 : HTTP 400.
- **Résultat obtenu** : Quantité 20 : HTTP 201 (commande créée). Quantité 21 : HTTP 400 « quantity must be between 1 and 20 ».
- **Preuve** : [TC-10.txt](evidence/TC-10.txt)

## TC-11 : Quantité demandée supérieure au stock restant

Type : Limite, priorité : Haute, statut : **Fail** (**BUG-04**)

- **Objectif** : Vérifier qu'on ne peut pas commander plus que le stock disponible.
- **Préconditions** : Client connecté ; Salmon Bowl (Tokyo Bowl) stock = 2.
- **Données** : Salmon Bowl quantité 3
- **Étapes** :
  1. Ajouter Salmon Bowl qté 3 au panier
  2. Passer commande par carte
  3. Consulter le stock (GET /api/products/{id})
- **Résultat attendu** : L'ajout ou la commande est refusé (stock insuffisant) ; le stock ne devient jamais négatif.
- **Résultat obtenu** : Commande #7 confirmée et payée (total 51,19 €) ; le stock de Salmon Bowl passe de 2 à -1.
- **Preuve** : [TC-11-1-ajout-quantite-3-stock-2.png](evidence/png/TC-11-1-ajout-quantite-3-stock-2.png), [TC-11-2-commande.png](evidence/png/TC-11-2-commande.png), [TC-11-3-stock-negatif.png](evidence/png/TC-11-3-stock-negatif.png), [TC-11.txt](evidence/TC-11.txt)

## TC-12 : Commande sous le minimum du restaurant

Type : Limite, priorité : Haute, statut : **Fail** (**BUG-06**)

- **Objectif** : Vérifier que le minimum de commande affiché est appliqué.
- **Préconditions** : Client connecté ; Burger Factory minimum 20,00 €.
- **Données** : Classic Burger ×1 + Frites ×1 = 15,40 €
- **Étapes** :
  1. Remplir le panier
  2. Checkout
  3. Confirmer par carte
- **Résultat attendu** : Commande refusée avec un message indiquant le minimum de 20,00 €.
- **Résultat obtenu** : Le checkout affiche « Commande minimum : 20.00 € » mais la commande #8 est confirmée avec un total de 18,39 €.
- **Preuve** : [TC-12-1-checkout-minimum.png](evidence/png/TC-12-1-checkout-minimum.png), [TC-12-2-commande.png](evidence/png/TC-12-2-commande.png)

## TC-13 : Appel de l'API sans clé

Type : Erreur, priorité : Haute, statut : **Pass**

- **Objectif** : Vérifier que l'API est protégée.
- **Préconditions** : -
- **Données** : Pas d'en-tête X-API-Key
- **Étapes** :
  1. GET /restaurants sans clé
- **Résultat attendu** : HTTP 401 avec message d'erreur JSON.
- **Résultat obtenu** : HTTP 401 ; « missing or invalid X-API-Key ».
- **Preuve** : [TC-13.txt](evidence/TC-13.txt)

## TC-14 : product_id envoyé en texte à l'API

Type : Erreur, priorité : Moyenne, statut : **Fail** (**BUG-09**)

- **Objectif** : Vérifier la robustesse face à un type inattendu.
- **Préconditions** : Clé API valide.
- **Données** : "product_id": "<id Margherita>" (string)
- **Étapes** :
  1. POST /orders avec product_id en chaîne
- **Résultat attendu** : HTTP 400 (validation) ou 201 (conversion), jamais d'erreur serveur 500.
- **Résultat obtenu** : HTTP 500, réponse au format text/html.
- **Preuve** : [TC-14.txt](evidence/TC-14.txt)

## TC-15 : Connexion avec mauvais mot de passe

Type : Erreur, priorité : Haute, statut : **Pass**

- **Objectif** : Vérifier le refus d'authentification.
- **Préconditions** : Compte alice existant.
- **Données** : alice@example.com / mauvais
- **Étapes** :
  1. Se connecter avec un mauvais mot de passe
- **Résultat attendu** : Message « Email ou mot de passe incorrect. » ; non connecté.
- **Résultat obtenu** : Message « Email ou mot de passe incorrect. » ; utilisateur non connecté.
- **Preuve** : [TC-15-mauvais-mot-de-passe.png](evidence/png/TC-15-mauvais-mot-de-passe.png)

## TC-16 : Coupon expiré EXPIRED10

Type : Erreur, priorité : Haute, statut : **Pass**

- **Objectif** : Vérifier le refus d'un coupon expiré (API et UI).
- **Préconditions** : Coupon EXPIRED10 expiré depuis 1 jour.
- **Données** : code EXPIRED10 ; subtotal 50
- **Étapes** :
  1. POST /coupons/validate
  2. Checkout UI avec EXPIRED10
- **Résultat attendu** : API : HTTP 404, valid=false ; UI : message « Coupon invalide ou expiré. », réduction 0.
- **Résultat obtenu** : API : HTTP 404, valid = false, « Coupon invalide ou expiré. ». Interface : même message, réduction 0,00 €.
- **Preuve** : [TC-16-coupon-expire.png](evidence/png/TC-16-coupon-expire.png), [TC-16.txt](evidence/TC-16.txt)

## TC-17 : Accès à la commande d'un autre client

Type : Erreur, priorité : Haute, statut : **Pass**

- **Objectif** : Vérifier le cloisonnement des commandes entre clients.
- **Préconditions** : Bob connecté ; commande seed #1 appartenant à Alice.
- **Données** : order_id de la commande d'Alice
- **Étapes** :
  1. Se connecter en Bob
  2. Ouvrir /orders/<id commande Alice>
- **Résultat attendu** : Accès refusé (« Commande inaccessible. ») ; aucune donnée d'Alice affichée.
- **Résultat obtenu** : Message « Commande inaccessible. » ; aucune donnée de la commande d'Alice affichée.
- **Preuve** : [TC-17-commande-autre-client.png](evidence/png/TC-17-commande-autre-client.png)

## TC-18 : Checkout avec panier vide

Type : Erreur, priorité : Moyenne, statut : **Pass**

- **Objectif** : Vérifier la redirection quand le panier est vide.
- **Préconditions** : Client connecté ; panier vide.
- **Données** : -
- **Étapes** :
  1. Vider le panier
  2. Ouvrir /checkout
- **Résultat attendu** : Redirection vers /cart avec « Votre panier est vide. ».
- **Résultat obtenu** : Redirection vers /cart avec le message « Votre panier est vide. ».
- **Preuve** : [TC-18-panier-vide.png](evidence/png/TC-18-panier-vide.png)

## TC-19 : Commande impossible dans un restaurant fermé

Type : Règle métier, priorité : Haute, statut : **Fail** (**BUG-03**)

- **Objectif** : Vérifier qu'un restaurant fermé ne reçoit pas de commande.
- **Préconditions** : Night Sushi is_open = false (affiché « FERMÉ »).
- **Données** : Late Sushi Box ×1 (14,00 €)
- **Étapes** :
  1. Ouvrir Night Sushi
  2. Ajouter Late Sushi Box
  3. Passer commande par carte
- **Résultat attendu** : Ajout au panier ou commande refusé avec un message « restaurant fermé ».
- **Résultat obtenu** : Le menu affiche « FERMÉ » mais l'ajout est accepté et la commande #9 est confirmée.
- **Preuve** : [TC-19-1-ajout-restaurant-ferme.png](evidence/png/TC-19-1-ajout-restaurant-ferme.png), [TC-19-2-commande.png](evidence/png/TC-19-2-commande.png)

## TC-20 : Paiement en espèces avec un montant insuffisant

Type : Règle métier, priorité : Haute, statut : **Fail** (**BUG-02**)

- **Objectif** : Vérifier le refus d'un paiement espèces inférieur au total.
- **Préconditions** : Client connecté.
- **Données** : Pizza Roma : total 24,29 € ; espèces remises 1,00 €
- **Étapes** :
  1. Remplir le panier
  2. Checkout, Espèces, montant 1
  3. Confirmer
- **Résultat attendu** : Commande refusée : « montant insuffisant ».
- **Résultat obtenu** : Commande #10 confirmée, paiement paid ; « Monnaie calculée : -23.29 € ».
- **Preuve** : [TC-20-1-checkout-especes-1-euro.png](evidence/png/TC-20-1-checkout-especes-1-euro.png), [TC-20-2-commande.png](evidence/png/TC-20-2-commande.png)

## TC-21 : Une seule commande créée lors d'un paiement PayPal

Type : Règle métier, priorité : Critique, statut : **Fail** (**BUG-01**)

- **Objectif** : Vérifier qu'un paiement PayPal crée exactement une commande.
- **Préconditions** : Client Alice connecté.
- **Données** : Pizza Roma : Margherita ×2 ; paiement PayPal
- **Étapes** :
  1. Compter les commandes (GET /api/orders)
  2. Commander via PayPal
  3. Recompter
- **Résultat attendu** : Exactement +1 commande ; un seul débit.
- **Résultat obtenu** : Nombre de commandes : 10 avant, 12 après. Deux commandes PayPal identiques à 24,29 € (#11 et #12).
- **Preuve** : [TC-21-1-checkout-paypal.png](evidence/png/TC-21-1-checkout-paypal.png), [TC-21-2-deux-commandes.png](evidence/png/TC-21-2-deux-commandes.png), [TC-21.txt](evidence/TC-21.txt)

## TC-22 : Promotion ATLAS15 annoncée à -15 %

Type : Règle métier, priorité : Haute, statut : **Fail** (**BUG-08**)

- **Objectif** : Vérifier que la réduction appliquée correspond à la promotion affichée.
- **Préconditions** : Couscous Atlas affiche « utilisez le code ATLAS15 pour obtenir 15 % de réduction ».
- **Données** : Couscous Royal ×2 = 35,80 € ; code ATLAS15
- **Étapes** :
  1. Remplir le panier
  2. Checkout avec ATLAS15
- **Résultat attendu** : Réduction = 15 % × 35,80 = 5,37 €.
- **Résultat obtenu** : La page annonce « 15 % de réduction » ; réduction appliquée : 3,58 € sur 35,80 € (10 %).
- **Preuve** : [TC-22-1-offre-15-pourcent.png](evidence/png/TC-22-1-offre-15-pourcent.png), [TC-22-2-reduction-appliquee.png](evidence/png/TC-22-2-reduction-appliquee.png)

## TC-23 : Panier mono-restaurant

Type : Règle métier, priorité : Moyenne, statut : **Pass**

- **Objectif** : Vérifier qu'un panier ne mélange pas deux restaurants.
- **Préconditions** : Panier contenant un produit Burger Factory.
- **Données** : Classic Burger puis Margherita
- **Étapes** :
  1. Ajouter Classic Burger
  2. Ajouter Margherita
- **Résultat attendu** : Second ajout refusé : « Un panier ne peut contenir que des produits du même restaurant. ».
- **Résultat obtenu** : Message « Un panier ne peut contenir que des produits du même restaurant. » ; Margherita n'est pas dans le panier.
- **Preuve** : [TC-23-1-refus-autre-restaurant.png](evidence/png/TC-23-1-refus-autre-restaurant.png), [TC-23-2-panier.png](evidence/png/TC-23-2-panier.png)

## TC-24 : Adresse de livraison exigée pour un nouveau compte

Type : Règle métier, priorité : Haute, statut : **Fail** (**BUG-05**)

- **Objectif** : Vérifier qu'une commande ne peut pas être livrée à une adresse vide.
- **Préconditions** : Compte nouvellement créé (aucune adresse saisie).
- **Données** : Margherita ×2
- **Étapes** :
  1. Créer un compte
  2. Ajouter au panier
  3. Checkout et confirmer par carte
- **Résultat attendu** : Le checkout exige une adresse complète ; aucune commande créée sans rue / code postal / ville.
- **Résultat obtenu** : Adresse proposée : « Domicile — , » (vide). La commande #13 est confirmée avec une adresse de livraison vide.
- **Preuve** : [TC-24-1-checkout-adresse-vide.png](evidence/png/TC-24-1-checkout-adresse-vide.png), [TC-24-2-commande-sans-adresse.png](evidence/png/TC-24-2-commande-sans-adresse.png)
