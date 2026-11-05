# Bilan de recette — Saleor Demo Store

> **Document provisoire.** La campagne d'exécution (« Exécution 1 - Chrome desktop - 05/11/2026 » dans Xray) n'est pas terminée. Les cases marquées _à compléter_ seront remplies à partir du rapport Xray. Aucun chiffre n'est estimé.

## Verdict : **NO-GO (provisoire)**

1. **Pourquoi** : l'inscription est impossible (BUG-1, gravité proposée « bloquante »), ce qui ne respecte pas le critère de sortie du plan de test sur les parcours critiques.
2. **Ce qui fonctionne** : catalogue, recherche, panier et validations du formulaire de caisse se comportent correctement en découverte.
3. **Réserves** : la campagne est en cours et BUG-1 reste à confirmer avec un second email. Le verdict peut changer à la fin de l'exécution.

## Résultats par fonctionnalité

À compléter à la fin de la campagne, à partir du rapport Xray.

| Fonctionnalité | Cas | Passed | Failed | Blocked | Not run |
|---|---|---|---|---|---|
| Compte | 8 | _à compléter_ | _à compléter_ | _à compléter_ | _à compléter_ |
| Recherche | 5 | _à compléter_ | _à compléter_ | _à compléter_ | _à compléter_ |
| Catalogue | 5 | _à compléter_ | _à compléter_ | _à compléter_ | _à compléter_ |
| Panier | 9 | _à compléter_ | _à compléter_ | _à compléter_ | _à compléter_ |
| Commande | 14 | _à compléter_ | _à compléter_ | _à compléter_ | _à compléter_ |
| **Total** | **41** | _à compléter_ | _à compléter_ | _à compléter_ | _à compléter_ |

## Anomalies, classées par gravité

Détail, étapes de reproduction et captures dans [anomalies.md](anomalies.md). La gravité est celle proposée, et aucune anomalie n'a encore été confirmée par une seconde reproduction.

| Gravité | Réf. | Anomalie | Statut |
|---|---|---|---|
| Bloquante | BUG-1 | Création de compte impossible (HTTP 400, `redirectUrl` refusée) | À confirmer avec un second email |
| Majeure | BUG-6 | Sous-total du panier incohérent avec la caisse | À reproduire en navigation privée |
| Majeure ou Mineure | BUG-2 | Message « Failed to create account » sans explication | À trancher |
| Mineure | BUG-4 | Frais de livraison annoncés « Free » avant leur calcul | À reproduire avec un panier neuf |
| Mineure | BUG-3 | Code HTML `&nbsp;` visible sur la page Homewares | Observée une fois |
| Mineure | BUG-5 | Libellés « (optional) » doublés | À confirmer |

## Cas bloqués et justification

Cas qui ne peuvent pas être exécutés normalement sur cet environnement. Ils sont proposés comme Blocked, à confirmer à l'exécution.

| Cas | Titre | Justification |
|---|---|---|
| TC-ACC-02 | Inscription avec un email déjà utilisé | Aucun compte ne peut être créé (BUG-1), donc aucun email n'existe déjà |
| TC-ACC-05 | Connexion avec identifiants valides | Aucun compte disponible (BUG-1) |
| TC-ACC-06 | Connexion avec un mot de passe incorrect | Aucun compte disponible (BUG-1) |
| TC-ACC-07 | Déconnexion | Aucune session ouvrable sans compte (BUG-1) |
| TC-CMD-12 | Paiement de la commande | Aucune passerelle de paiement configurée, bouton « Pay » inactif |

## Lien avec les critères Go / No-Go du plan de test

Critères définis dans le [plan de test](plan-de-test.md) avant l'exécution.

| Critère | Seuil | Situation actuelle | Verdict du critère |
|---|---|---|---|
| Cas exécutés | 100 % | Campagne en cours | Non atteint |
| Cas Passed | au moins 90 % | À calculer à la fin de la campagne | Non évaluable |
| Anomalie bloquante ou critique ouverte sur un parcours critique | aucune | BUG-1 ouverte (inscription impossible) | **Non respecté** |

Le plan prévoit un **No-Go** dès qu'une anomalie bloquante ou critique reste ouverte sur un parcours critique. BUG-1 suffit à déclencher ce critère, sous réserve de sa confirmation.

Pour passer à Go : confirmer ou infirmer BUG-1, la faire corriger puis rejouer les cas du compte, traiter BUG-6 si elle est confirmée, terminer les 41 cas et atteindre au moins 90 % de Passed.

## Limites et risques résiduels

**Limites de la recette**
- Le site testé est une démonstration publique : l'inscription échoue et aucune passerelle de paiement n'est configurée, donc les cas de connexion, de déconnexion et de paiement sont bloqués.
- Un seul navigateur (Chrome, desktop), aucun test de performance, de sécurité ni d'accessibilité.
- Plusieurs anomalies n'ont pas encore été reproduites une seconde fois.

**Risques résiduels**
- **Fonctions non couvertes** : sans compte, tout ce qui dépend d'un utilisateur connecté (profil, historique de commandes) n'est pas testé. Des défauts peuvent s'y cacher.
- **Paiement non testé** : le résultat de la recette ne dit rien sur la confirmation d'une commande.
- **Montants du panier** : si BUG-6 est confirmée, des montants et un message de livraison gratuite faux peuvent être affichés au client.
- **Environnement instable** : la démo peut être modifiée ou réinitialisée, ce qui rendrait des résultats non reproductibles. Les captures datées servent de preuve.

## Prochaines actions

- Terminer l'exécution dans Xray, compléter les tableaux et corriger les statuts incohérents.
- Confirmer BUG-1 avec un second email et reproduire BUG-4 et BUG-6.
- Réaliser 2 à 3 sessions exploratoires et les ajouter au dépôt.
- Créer les tickets Bug dans Jira et les lier aux cas en échec.
- Finaliser ce bilan avec les chiffres de la campagne.
