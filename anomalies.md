# Anomalies détectées — Recette Saleor Demo Store

Environnement : https://storefront.saleor.io (boutique de démonstration publique), Chrome, Windows, 05/11/2026.

> Ces anomalies ont été relevées pendant la phase de **découverte**. Elles seront confirmées ou infirmées lors de la campagne d'exécution officielle. Les captures ont été masquées (données personnelles remplacées par des rectangles noirs).

**Origine** : *Produit* (défaut de l'application elle-même), *Environnement* (configuration ou contenu de la démo testée) ou *À déterminer*. Elle compte pour le verdict : un défaut d'environnement ne dit pas la même chose sur le produit qu'un défaut de code.

| Réf. | Gravité | Origine | Statut |
|---|---|---|---|
| BUG-1 | Bloquante | Environnement probable | À confirmer |
| BUG-6 | Majeure | Produit (hypothèse) | À reproduire |
| BUG-2 | Mineure | Produit | À confirmer |
| BUG-4 | Mineure | Produit | À reproduire |
| BUG-3 | Mineure | À déterminer | Observée une fois |
| BUG-5 | Mineure | Produit | À confirmer |

---

## BUG-1 — Création de compte impossible (HTTP 400)
- **Gravité** : Bloquante
- **Origine** : Environnement probable. L'erreur désigne le champ `redirectUrl`, pas les données saisies, ce qui ressemble à une configuration de la démo (URL de redirection du storefront non autorisée côté serveur). Hypothèse non confirmée : je n'ai pas accès à la configuration.
- **Cas lié** : TC-ACC-01
- **Étapes** : 1. Ouvrir « Create an Account » 2. Remplir tous les champs avec des données valides 3. Cliquer sur « Create Account »
- **Attendu** : le compte est créé
- **Obtenu** : bandeau « Failed to create account ». La requête `POST /api/auth/register` renvoie 400 Bad Request avec `field: redirectUrl`, `code: INVALID` et un message indiquant que l'URL de redirection du login n'est pas autorisée
- **Fréquence** : à confirmer avec un second email lors de la campagne
- **Captures** : [formulaire](captures/01-inscription-echec-formulaire.png) · [requête](captures/02-inscription-requete-register-400.png) · [réponse](captures/03-inscription-reponse-redirecturl.png)

## BUG-2 — Message d'échec d'inscription sans explication
- **Gravité** : Mineure. Ce défaut ne bloque rien à lui seul : la fonction qui échoue est couverte par BUG-1, et BUG-2 ne concerne que la qualité du message. Cela relève du confort dans l'échelle du plan de test. On peut soutenir qu'un message muet sur un parcours critique mérite mieux, mais la gravité du parcours est déjà portée par BUG-1.
- **Origine** : Produit. Le serveur renvoie une réponse détaillée que l'interface n'affiche pas.
- **Cas lié** : TC-ACC-08
- **Obtenu** : l'interface affiche uniquement « Failed to create account »
- **Attendu** : un message exploitable par l'utilisateur
- **Capture** : [formulaire](captures/01-inscription-echec-formulaire.png)

## BUG-3 — Code HTML « &nbsp; » visible sur la page Homewares
- **Gravité** : Mineure
- **Origine** : À déterminer. Soit la description de la catégorie a été saisie avec l'entité HTML (donnée de démo), soit le storefront n'interprète pas cette entité à l'affichage (produit).
- **Cas lié** : TC-CAT-05
- **Obtenu** : description « Everything a programmer's comfort requires&nbsp; »
- **Attendu** : phrase sans caractère parasite
- **Capture** : [Homewares](captures/07-categorie-homewares-nbsp.png)

## BUG-4 — Frais de livraison annoncés « Free » avant leur calcul
- **Gravité** : Mineure
- **Origine** : Produit (libellé du récapitulatif de commande)
- **Cas lié** : TC-CMD-08
- **Obtenu** : le récapitulatif affiche « Shipping : Free » (total 3,98 $) aux étapes Information et Shipping, puis les frais réels apparaissent à l'étape Payment (de 40,39 $ à 77,48 $ selon le mode)
- **Attendu** : un libellé neutre tant que le mode n'est pas confirmé, comme « Calculated at checkout » dans le panier
- **À faire** : reproduire avec un panier neuf et noter l'affichage à chaque étape
- **Captures** : [étape 1](captures/16-caisse-etape1-shipping-free.png) · [étape 2](captures/17-caisse-etape2-shipping-free.png) · [étape 3](captures/18-caisse-etape3-total-ace.png)

## BUG-5 — Libellés « (optional) » doublés (à confirmer)
- **Gravité** : Mineure
- **Origine** : Produit (libellés du formulaire de caisse)
- **Cas lié** : TC-CMD-10
- **Obtenu** : « Company (optional) (optional) », « Phone (optional) (optional) » sur le formulaire de caisse
- **Capture** : [formulaire de caisse](captures/27-caisse-libelles-optional.png)

## BUG-6 — Sous-total du panier incohérent avec la caisse (à reproduire)
- **Gravité** : Majeure
- **Origine** : Produit (calcul ou affichage du panier), hypothèse à confirmer
- **Cas liés** : TC-PAN-08, TC-PAN-09
- **Constat** : pour 6 Carrot Juice (1,99 $ l'unité), la caisse affiche un sous-total de 11,94 $ et une livraison de 63,94 $ (total 75,88 $). Le volet panier affiche un sous-total de 75,88 $, le message « You qualify for free shipping! » et « Shipping : Free »
- **Attendu** : sous-total de 11,94 $ et message indiquant le montant restant avant la livraison gratuite
- **Hypothèse sur la cause** : le sous-total du panier (75,88 $) est exactement le total de la caisse (11,94 + 63,94). Le panier semble donc afficher le total livraison comprise à la place du sous-total. Le seuil de livraison gratuite (75 $) paraît alors dépassé, ce qui expliquerait le message « free shipping » et « Shipping : Free ».
- **Seuil de 75 $** : relevé sur le site, pas supposé. Le bandeau annonce « Free shipping on orders over $75.00 » ([capture 12](captures/12-fiche-produit.png)), et le panier affiche « Add $73.01 more for free shipping » pour 1,99 $ de produits ([capture 13](captures/13-panier-1-article.png)). Les montants suivants confirment le calcul : 71,02 $ pour 3,98 $ ([capture 14](captures/14-panier-quantite-2.png)) et 69,03 $ pour 5,97 $ ([capture 15](captures/15-panier-2-produits.png)), soit 75,00 $ à chaque fois. À relire sur le site pendant la reproduction.
- **Réserve** : l'ordre exact des actions et des captures n'a pas été vérifié, donc l'écart pourrait avoir une autre cause.
- **Reproduction à faire** (navigation privée) :
  1. Ajouter 6 Carrot Juice, ouvrir le panier : le sous-total doit être de 11,94 $ (capture).
  2. Aller en caisse, choisir « fastway couriers » (63,94 $) et atteindre l'étape Payment.
  3. Revenir au panier : le sous-total passe-t-il à 75,88 $ ? (capture).
  4. Variante : recommencer avec ACE (40,39 $). L'hypothèse prédit alors un sous-total de 52,33 $ et aucun message de livraison gratuite (52,33 $ est sous le seuil de 75 $).
- **Captures** : [panier](captures/25-panier-sous-total-75-88.png) · [caisse](captures/26-caisse-sous-total-11-94.png)

---

## Remarques (pas de ticket)
- Aucune passerelle de paiement n'est configurée : le paiement n'est pas testable (statut Blocked). [Capture](captures/19-caisse-etape3-total-fastway.png)
- Le bandeau de l'étape Payment s'adresse à l'administrateur de la boutique, pas au client (TC-CMD-11).
- Le panier de caisse est retrouvé dans un autre navigateur à partir de l'URL seule, ce qui est un fonctionnement courant. Point de vigilance pour la confidentialité.
- La page de recherche affiche deux compteurs de produits (« 4 products found » et « 4 products »).
