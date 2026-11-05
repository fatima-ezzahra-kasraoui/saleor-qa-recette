# Anomalies détectées — Recette Saleor Demo Store

Environnement : https://storefront.saleor.io (boutique de démonstration publique), Chrome, Windows, 05/11/2026.

> Ces anomalies ont été relevées pendant la phase de **découverte**. Elles seront confirmées ou infirmées lors de la campagne d'exécution officielle. Les captures ont été masquées (données personnelles remplacées par des rectangles noirs).

---

## BUG-1 — Création de compte impossible (HTTP 400)
- **Gravité** : Bloquante
- **Cas lié** : TC-ACC-01
- **Étapes** : 1. Ouvrir « Create an Account » 2. Remplir tous les champs avec des données valides 3. Cliquer sur « Create Account »
- **Attendu** : le compte est créé
- **Obtenu** : bandeau « Failed to create account ». La requête `POST /api/auth/register` renvoie 400 Bad Request avec `field: redirectUrl`, `code: INVALID` et un message indiquant que l'URL de redirection du login n'est pas autorisée
- **Fréquence** : à confirmer avec un second email lors de la campagne
- **Piste** (hypothèse, non confirmée) : configuration serveur de la démo
- **Captures** : [formulaire](captures/01-inscription-echec-formulaire.png) · [requête](captures/02-inscription-requete-register-400.png) · [réponse](captures/03-inscription-reponse-redirecturl.png)

## BUG-2 — Message d'échec d'inscription sans explication
- **Gravité** : Majeure ou Mineure (à trancher)
- **Cas lié** : TC-ACC-08
- **Obtenu** : l'interface affiche uniquement « Failed to create account » alors que le serveur renvoie une réponse détaillée
- **Attendu** : un message exploitable par l'utilisateur
- **Capture** : [formulaire](captures/01-inscription-echec-formulaire.png)

## BUG-3 — Code HTML « &nbsp; » visible sur la page Homewares
- **Gravité** : Mineure
- **Cas lié** : TC-CAT-05
- **Obtenu** : description « Everything a programmer's comfort requires&nbsp; »
- **Attendu** : phrase sans caractère parasite
- **Capture** : [Homewares](captures/07-categorie-homewares-nbsp.png)

## BUG-4 — Frais de livraison annoncés « Free » avant leur calcul
- **Gravité** : Mineure
- **Cas lié** : TC-CMD-08
- **Obtenu** : le récapitulatif affiche « Shipping : Free » (total 3,98 $) aux étapes Information et Shipping, puis les frais réels apparaissent à l'étape Payment (de 40,39 $ à 77,48 $ selon le mode)
- **Attendu** : un libellé neutre tant que le mode n'est pas confirmé, comme « Calculated at checkout » dans le panier
- **À faire** : reproduire avec un panier neuf et noter l'affichage à chaque étape
- **Captures** : [étape 1](captures/16-caisse-etape1-shipping-free.png) · [étape 2](captures/17-caisse-etape2-shipping-free.png) · [étape 3](captures/18-caisse-etape3-total-ace.png)

## BUG-5 — Libellés « (optional) » doublés (à confirmer)
- **Gravité** : Mineure
- **Cas lié** : TC-CMD-10
- **Obtenu** : « Company (optional) (optional) », « Phone (optional) (optional) » sur le formulaire de caisse
- **Capture** : [formulaire de caisse](captures/27-caisse-libelles-optional.png)

## BUG-6 — Sous-total du panier incohérent avec la caisse (à reproduire)
- **Gravité** : Majeure
- **Cas liés** : TC-PAN-08, TC-PAN-09
- **Constat** : pour 6 Carrot Juice (1,99 $ l'unité), la caisse affiche un sous-total de 11,94 $ et une livraison de 63,94 $ (total 75,88 $). Le volet panier affiche un sous-total de 75,88 $, le message « You qualify for free shipping! » et « Shipping : Free »
- **Attendu** : sous-total de 11,94 $ et message indiquant le montant restant avant la livraison gratuite
- **Réserve** : l'ordre exact des captures n'a pas été vérifié. À reproduire en navigation privée (panier, caisse jusqu'au paiement, retour au panier) avant déclaration
- **Captures** : [panier](captures/25-panier-sous-total-75-88.png) · [caisse](captures/26-caisse-sous-total-11-94.png)

---

## Remarques (pas de ticket)
- Aucune passerelle de paiement n'est configurée : le paiement n'est pas testable (statut Blocked). [Capture](captures/19-caisse-etape3-total-fastway.png)
- Le bandeau de l'étape Payment s'adresse à l'administrateur de la boutique, pas au client (TC-CMD-11).
- Le panier de caisse est retrouvé dans un autre navigateur à partir de l'URL seule, ce qui est un fonctionnement courant. Point de vigilance pour la confidentialité.
- La page de recherche affiche deux compteurs de produits (« 4 products found » et « 4 products »).
