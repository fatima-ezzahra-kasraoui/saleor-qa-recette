# Recette fonctionnelle d'une boutique e-commerce (Saleor Demo Store)

Projet personnel de QA : recette manuelle et exploratoire d'une application e-commerce open source, réalisée comme en entreprise (plan de test, cas de test dans un outil de gestion de tests, anomalies, bilan).

- **Produit testé** : [Saleor Demo Store](https://storefront.saleor.io), boutique de démonstration publique
- **Date de début** : 5 novembre 2026
- **Outils** : Xray (essai gratuit, dans Jira Cloud), Jira, Chrome DevTools, GitHub

## État d'avancement

| Étape | Statut |
|---|---|
| Plan de test | Fait ([plan-de-test.md](plan-de-test.md)) |
| Découverte du produit | Faite : 6 anomalies candidates ([anomalies.md](anomalies.md)) |
| Conception : 41 cas de test | Faite ([cas-de-test.csv](cas-de-test.csv)), importés dans Xray |
| Campagne d'exécution dans Xray | À venir |
| Sessions exploratoires (2 à 3) | À venir |
| Bilan de recette Go/No-Go | À venir |

Ce dépôt est mis à jour au fil du projet. Les résultats de la découverte ne sont **pas** des résultats de recette : chaque cas sera réexécuté officiellement, avec un statut (Passed, Failed, Blocked) et des preuves.

## Périmètre

Trois fonctionnalités :
1. **Compte utilisateur** : inscription, connexion, déconnexion
2. **Recherche et catalogue** : recherche, catégories, filtres, fiche produit
3. **Panier et début de commande** : ajout, quantité, suppression, passage en caisse jusqu'à l'étape de paiement

Hors périmètre : paiement réel, tests de performance et de sécurité, administration, automatisation.

## Méthode

- **Tests scriptés** : 41 cas (nominaux, limites, erreurs, ergonomie), une ligne par étape avec résultat attendu
- **Test exploratoire** : sessions chartées de 45 minutes (session-based testing)
- **Gravité des anomalies** : Bloquante, Critique, Majeure, Mineure (définitions dans le plan de test)
- **Critères Go/No-Go** : définis avant l'exécution dans le plan de test

## Premiers constats (découverte, non officiels)

| Réf. | Anomalie | Gravité |
|---|---|---|
| BUG-1 | Création de compte impossible : la requête d'inscription renvoie HTTP 400 (`redirectUrl` refusée) | Bloquante |
| BUG-2 | Message « Failed to create account » sans explication | Majeure ou Mineure |
| BUG-3 | Code HTML `&nbsp;` visible sur la page Homewares | Mineure |
| BUG-4 | Frais de livraison annoncés « Free » avant leur calcul | Mineure |
| BUG-5 | Libellés « (optional) » doublés sur le formulaire de caisse (à confirmer) | Mineure |
| BUG-6 | Sous-total du panier incohérent avec la caisse (à reproduire) | Majeure |

Le détail, les étapes de reproduction et les captures sont dans [anomalies.md](anomalies.md).

Limites de l'environnement : l'inscription échoue (les cas de connexion seront probablement Blocked) et aucune passerelle de paiement n'est configurée (cas de paiement Blocked).

## Contenu du dépôt

```
.
├── README.md
├── plan-de-test.md     Plan de test (périmètre, approche, critères, risques)
├── cas-de-test.csv     41 cas de test avec notes de découverte
├── anomalies.md        Anomalies avec étapes de reproduction et captures
└── captures/           Captures d'écran (données personnelles masquées)
```

## Index des captures

| Zone testée | Captures |
|---|---|
| Inscription (BUG-1, BUG-2) | [01](captures/01-inscription-echec-formulaire.png), [02](captures/02-inscription-requete-register-400.png), [03](captures/03-inscription-reponse-redirecturl.png), [29](captures/29-inscription-liste-requetes.png) |
| Catalogue et filtres | [04](captures/04-catalogue-all-products.png), [05](captures/05-filtre-categorie-tshirts.png), [06](captures/06-categorie-groceries.png), [07](captures/07-categorie-homewares-nbsp.png) |
| Recherche | [08](captures/08-recherche-juice.png), [09](captures/09-recherche-sans-resultat.png), [10](captures/10-recherche-caracteres-speciaux.png), [11](captures/11-recherche-casse.png) |
| Fiche produit et panier | [12](captures/12-fiche-produit.png), [13](captures/13-panier-1-article.png), [14](captures/14-panier-quantite-2.png), [15](captures/15-panier-2-produits.png) |
| Caisse, étapes (BUG-4) | [16](captures/16-caisse-etape1-shipping-free.png), [28](captures/28-caisse-etape1-formulaire-rempli.png), [17](captures/17-caisse-etape2-shipping-free.png), [31](captures/31-caisse-etape2-fastway-selectionne.png), [18](captures/18-caisse-etape3-total-ace.png), [19](captures/19-caisse-etape3-total-fastway.png) |
| Caisse, validations | [20 téléphone invalide](captures/20-caisse-telephone-invalide.png), [30 téléphone étranger accepté](captures/30-caisse-telephone-etranger-accepte.png), [21 code postal invalide](captures/21-caisse-code-postal-invalide.png), [22 email vide](captures/22-caisse-email-vide.png), [23 email invalide](captures/23-caisse-email-invalide.png), [24 code promo invalide](captures/24-caisse-code-promo-invalide.png), [27 libellés optional](captures/27-caisse-libelles-optional.png) |
| Totaux panier / caisse (BUG-6) | [25](captures/25-panier-sous-total-75-88.png), [26](captures/26-caisse-sous-total-11-94.png), [32](captures/32-caisse-6-articles-total-75-88.png) |
| Import des cas de test dans Xray | [33 mapping des champs](captures/33-xray-mapping-champs.png), [34 import réussi](captures/34-xray-import-reussi.png) |

## À propos des captures

Les données personnelles visibles lors des tests (nom, email, adresse, téléphone) ont été masquées par des rectangles noirs avant publication.
