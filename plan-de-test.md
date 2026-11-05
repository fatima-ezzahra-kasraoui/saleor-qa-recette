# Plan de test — Recette de la boutique démo Saleor

> Projet personnel de recette fonctionnelle (manuelle + exploratoire).
> Statut : v0.2 — phase de découverte terminée le 05/11/2026 (voir section 11). Prochaine étape : import dans Xray et campagne d'exécution.

## 1. Contexte et objectif
Réaliser une recette fonctionnelle d'une application e-commerce open source (Saleor, storefront de démonstration publique) comme on le ferait en entreprise : conception de cas de test, exécution, exploration manuelle, déclaration d'anomalies et bilan Go/No-Go.

Objectif : valider que les parcours critiques d'un client (trouver un produit, remplir un panier, se connecter, passer commande jusqu'à l'étape de paiement) fonctionnent et détecter les défauts de comportement, d'ergonomie et de robustesse.

## 2. Produit testé
- Application : Saleor Demo Store (storefront public)
- URL : https://storefront.saleor.io
- Version / date de test : à noter au premier jour d'exécution (ex. 2026-11-XX)
- Environnement : navigateur Chrome (dernière version), desktop + une passe mobile (émulation DevTools)

## 3. Périmètre
**Dans le périmètre (3 fonctionnalités)**
1. Compte utilisateur : inscription, connexion, déconnexion
2. Recherche et navigation catalogue : recherche, catégories, fiche produit
3. Panier et début de commande : ajout, modification de quantité, suppression, passage en caisse

**Hors périmètre**
- Paiement réel (on s'arrête avant toute transaction)
- Tests de performance, de sécurité (pentest), d'API GraphQL
- Dashboard d'administration
- Automatisation

## 4. Approche de test
| Type | Description |
|---|---|
| Tests fonctionnels scriptés | Cas de test rédigés (nominal, limites, erreurs) exécutés lors de la campagne |
| Test exploratoire | Sessions chartées de 45 min (session-based testing) |
| Test de compatibilité léger | Chrome desktop + vue mobile émulée |
| Test d'ergonomie de base | Messages d'erreur, libellés, cohérence |

## 5. Outils
- Gestion de tests : Xray (Jira Cloud, essai 30 jours)
- Anomalies : Jira
- Outils d'investigation : Chrome DevTools (console, réseau)
- Documentation : GitHub (ce dépôt)

## 6. Critères d'entrée / sortie
**Entrée** : site accessible, plan de test validé, cas de test rédigés, compte de test créé.
**Sortie (Go)** : 100 % des cas exécutés, 0 anomalie bloquante ou critique ouverte sur les parcours critiques, au moins 90 % de cas « Passed ».
**No-Go** : au moins une anomalie bloquante ou critique ouverte sur un parcours critique.

## 7. Gravité des anomalies
| Niveau | Définition |
|---|---|
| Bloquante | Empêche un parcours critique, sans contournement |
| Critique | Fonction majeure dégradée, contournement difficile |
| Majeure | Fonction dégradée, contournement existe |
| Mineure | Défaut cosmétique ou de confort |

## 8. Risques
| Risque | Impact | Parade |
|---|---|---|
| Site de démo modifié ou réinitialisé pendant les tests | Résultats non reproductibles | Dater chaque exécution, prendre des captures |
| Fonction non disponible sur la démo | Cas non exécutable | Statut « Blocked » avec justification |
| Essai Xray limité à 30 jours | Perte d'accès aux preuves | Exporter rapports et captures dans le dépôt avant la fin |

## 9. Planning
| Jour | Activité |
|---|---|
| 1 | Découverte du produit (15-30 min), ajustement du périmètre, finalisation du plan |
| 2 | Rédaction des cas de test (document CSV) |
| 3 | Ouverture de l'essai Xray, import des cas, création du plan et de la campagne |
| 4 | Exécution de la campagne, déclaration des anomalies |
| 5 | Sessions exploratoires (2 à 3) |
| 6 | Bilan de recette, exports, README final |

## 10. Livrables
Plan de test, cas de test (Xray), rapport d'exécution, notes de sessions exploratoires, anomalies (Jira), bilan Go/No-Go.

## 11. Constats de la phase de découverte (05/11/2026)
La découverte a servi à ajuster le périmètre et à écrire des cas réalistes. Ses résultats ne sont pas des résultats de recette : tous les cas seront réexécutés officiellement dans Xray.

**Ce qui est accessible**
- Catalogue (29 produits), catégories, filtres, recherche, fiche produit : accessibles
- Panier et caisse en invité (3 étapes : Information, Shipping, Payment) : accessibles

**Limites de l'environnement**
- L'inscription échoue (HTTP 400), donc les cas de connexion et de déconnexion sont probablement Blocked
- Aucune passerelle de paiement configurée : le cas de paiement est Blocked

**Anomalies candidates** : 6 (voir anomalies.md). BUG-1 est bloquante sur le parcours de compte, BUG-6 (sous-total du panier) est à reproduire avant déclaration.

**Ajustements du périmètre**
- Le code postal et le téléphone sont optionnels à la caisse : les cas de validation portent sur des valeurs absurdes, pas sur des champs vides
- Ajout de cas sur la cohérence des totaux panier / caisse et sur le libellé des frais de livraison

**Prochaines actions**
1. Reproduire BUG-4 et BUG-6 avec un panier neuf en navigation privée
2. Ouvrir l'essai Xray, importer test-cases.csv, créer le plan de test et la campagne
3. Exécuter la campagne, puis réaliser 2 à 3 sessions exploratoires chartées
4. Rédiger le bilan Go/No-Go
