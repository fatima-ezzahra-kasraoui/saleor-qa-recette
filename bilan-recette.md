# Bilan de recette — Saleor Demo Store

> **Statut : brouillon provisoire.** La campagne d'exécution (« Exécution 1 - Chrome desktop - 05/11/2026 » dans Xray) n'est pas terminée. Ce document applique les critères du [plan de test](plan-de-test.md) aux informations disponibles. Il sera finalisé quand tous les cas seront exécutés.

## Recommandation provisoire : **NO-GO**

La démo ne remplit pas aujourd'hui les critères de sortie définis dans le plan de test. Cette recommandation reste **conditionnée à la confirmation de BUG-1** (voir plus bas).

## Critères du plan de test

| Critère | Seuil | Situation actuelle |
|---|---|---|
| Cas exécutés | 100 % | Non atteint, campagne en cours |
| Cas « Passed » | au moins 90 % | Non évaluable avant la fin de la campagne |
| Anomalie bloquante ou critique ouverte sur un parcours critique | aucune | **Non respecté** : BUG-1 (inscription impossible), proposée comme bloquante |

Le plan prévoit un No-Go dès qu'une anomalie bloquante ou critique reste ouverte sur un parcours critique. C'est le cas avec BUG-1.

## Résultats de la campagne

À compléter à la fin de l'exécution, à partir du rapport Xray.

| Statut | Nombre de cas |
|---|---|
| Passed | _à compléter_ |
| Failed | _à compléter_ |
| Blocked | _à compléter_ |
| Non exécutés | _à compléter_ |
| **Total** | 41 |

## Anomalies

Détail, étapes de reproduction et captures dans [anomalies.md](anomalies.md).

| Réf. | Anomalie | Gravité proposée | Statut |
|---|---|---|---|
| BUG-1 | Création de compte impossible (HTTP 400, `redirectUrl` refusée) | Bloquante | À confirmer avec un second email |
| BUG-6 | Sous-total du panier incohérent avec la caisse | Majeure | À reproduire en navigation privée |
| BUG-2 | Message d'échec d'inscription sans explication | Majeure ou Mineure | À trancher |
| BUG-4 | Frais de livraison annoncés « Free » avant leur calcul | Mineure | À reproduire avec un panier neuf |
| BUG-3 | Code HTML `&nbsp;` visible sur la page Homewares | Mineure | Observée |
| BUG-5 | Libellés « (optional) » doublés | Mineure | À confirmer |

## Ce qui fonctionne (découverte, à confirmer par la campagne)

- Catalogue, catégories, filtres et fiche produit
- Recherche : résultat, absence de résultat, caractères spéciaux, insensibilité à la casse
- Panier : ajout, quantité, total de plusieurs produits, persistance après rechargement
- Validations du formulaire de caisse : email, téléphone, code postal, code promo

## Limites de la recette

- Le site testé est une démonstration publique : l'inscription échoue et aucune passerelle de paiement n'est configurée. Les cas de connexion, de déconnexion et de paiement sont donc bloqués, et le résultat de la recette ne dit rien sur ces fonctions.
- Un seul navigateur (Chrome, desktop) et aucun test de performance ni de sécurité.
- Certaines anomalies n'ont pas encore été reproduites une seconde fois.

## Conditions pour passer à Go

1. Confirmer ou infirmer BUG-1 (second email, nouvelle tentative).
2. Corriger BUG-1, puis rejouer les cas du parcours de compte.
3. Reproduire BUG-6 et la corriger si elle est confirmée.
4. Terminer l'exécution des 41 cas et retrouver au moins 90 % de cas Passed.

## Prochaines actions

- Terminer l'exécution dans Xray et compléter le tableau des résultats.
- Réaliser 2 à 3 sessions exploratoires et les ajouter au dépôt.
- Créer les tickets Bug dans Jira et les lier aux cas en échec.
- Finaliser ce bilan avec les chiffres de la campagne.
