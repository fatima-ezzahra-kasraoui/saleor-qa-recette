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

## À propos des captures

Les données personnelles visibles lors des tests (nom, email, adresse, téléphone) ont été masquées par des rectangles noirs avant publication.
