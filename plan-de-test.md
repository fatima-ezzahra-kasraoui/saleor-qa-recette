# Plan de test — Recette de la boutique démo Saleor

> Projet personnel de recette fonctionnelle (manuelle + exploratoire).
> Statut : **v0.3** (06/11/2026). Les cas de test sont importés dans Xray et la campagne d'exécution est en cours. Historique des changements en section 13.

## 1. Contexte et objectif
Réaliser une recette fonctionnelle d'une application e-commerce open source (Saleor, storefront de démonstration publique) comme on le ferait en entreprise : conception de cas de test, exécution, exploration manuelle, déclaration d'anomalies et bilan Go/No-Go.

Objectif : valider que les parcours critiques d'un client (trouver un produit, remplir un panier, se connecter, passer commande jusqu'à l'étape de paiement) fonctionnent et détecter les défauts de comportement, d'ergonomie et de robustesse.

## 2. Produit testé
- Application : Saleor Demo Store (storefront public)
- URL : https://storefront.saleor.io
- Date de début des tests : 05/11/2026
- Environnement : navigateur Chrome (dernière version), desktop. Une passe mobile émulée était prévue à l'origine et n'a pas été réalisée dans la campagne 1 (voir section 12).

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
| Test de compatibilité léger | Chrome desktop. La passe mobile émulée n'est pas réalisée dans la campagne 1 (voir section 12) |
| Test d'ergonomie de base | Messages d'erreur, libellés, cohérence |

## 5. Outils
- Gestion de tests : Xray (Jira Cloud, essai 30 jours)
- Anomalies : Jira
- Outils d'investigation : Chrome DevTools (console, réseau)
- Documentation : GitHub (ce dépôt)

## 6. Critères d'entrée / sortie
**Entrée** : site accessible, plan de test validé, cas de test rédigés, compte de test créé. *Le dernier point n'a pas pu être respecté : l'inscription échoue (voir section 12).*

**Sortie (Go)**, tous les points à la fois :
1. 100 % des cas **exécutables** exécutés. Les cas Blocked sont comptés à part, avec leur justification, et exclus du dénominateur.
2. Au moins 90 % de cas Passed **parmi les cas exécutés** (Blocked exclus).
3. Aucune anomalie bloquante ou critique ouverte sur un parcours critique.
4. Si tous les cas d'un parcours critique sont Blocked, le Go ne peut être que conditionnel : ce parcours n'est pas validé.

**No-Go** : au moins une anomalie bloquante ou critique ouverte sur un parcours critique.

> **Précision apportée en v0.3 (06/11/2026).** La première version demandait 100 % de cas exécutés et 90 % de Passed sans tenir compte des cas Blocked. Or avec 5 cas bloqués sur 41, le maximum atteignable serait 36/41 = 87,8 % : le critère était impossible à satisfaire. Les seuils (100 % et 90 %) sont inchangés, seul le dénominateur est précisé. Cette précision a été faite après les premiers constats ; le verdict provisoire ne dépend pas d'elle, car le point 3 et le critère No-Go suffisent à le déterminer.

## 7. Gravité des anomalies
| Niveau | Définition |
|---|---|
| Bloquante | Empêche un parcours critique, sans contournement |
| Critique | Fonction majeure dégradée, contournement difficile |
| Majeure | Fonction dégradée, contournement existe |
| Mineure | Défaut cosmétique ou de confort |

Chaque anomalie porte aussi une **origine** : produit, environnement de test, ou à déterminer. Un défaut d'environnement (par exemple une configuration de la démo) n'a pas la même portée qu'un défaut du produit.

## 8. Risques
| Risque | Impact | Parade |
|---|---|---|
| Site de démo modifié ou réinitialisé pendant les tests | Résultats non reproductibles | Dater chaque exécution, prendre des captures |
| Fonction non disponible sur la démo | Cas non exécutable | Statut « Blocked » avec justification |
| Essai Xray limité à 30 jours | Perte d'accès aux preuves | Exporter rapports et captures dans le dépôt avant la fin |

## 9. Planning
| Jour | Activité | Réalisation |
|---|---|---|
| 1 | Découverte du produit (15-30 min), ajustement du périmètre, finalisation du plan | Fait (05/11) |
| 2 | Rédaction des cas de test (document CSV) | Fait (05/11) |
| 3 | Ouverture de l'essai Xray, import des cas, création du plan et de la campagne | Fait (05/11) |
| 4 | Exécution de la campagne, déclaration des anomalies | En cours |
| 5 | Sessions exploratoires (2 à 3) | À venir |
| 6 | Bilan de recette, exports, README final | Bilan provisoire rédigé |

## 10. Livrables
Plan de test, cas de test (Xray), rapport d'exécution, notes de sessions exploratoires, anomalies (Jira), bilan Go/No-Go.

## 11. Constats de la phase de découverte (05/11/2026)
La découverte a servi à ajuster le périmètre et à écrire des cas réalistes. Ses résultats ne sont pas des résultats de recette : tous les cas sont réexécutés officiellement dans Xray.

**Ce qui est accessible**
- Catalogue (29 produits), catégories, filtres, recherche, fiche produit : accessibles
- Panier et caisse en invité (3 étapes : Information, Shipping, Payment) : accessibles

**Limites de l'environnement**
- L'inscription échoue (HTTP 400), donc les cas de connexion et de déconnexion sont probablement Blocked
- Aucune passerelle de paiement configurée : le cas de paiement est Blocked

**Anomalies candidates** : 6 (voir [anomalies.md](anomalies.md)). BUG-1 est bloquante sur le parcours de compte, BUG-6 (sous-total du panier) est à reproduire avant déclaration.

**Ajustements du périmètre**
- Le code postal et le téléphone sont optionnels à la caisse : les cas de validation portent sur des valeurs absurdes, pas sur des champs vides
- Ajout de cas sur la cohérence des totaux panier / caisse et sur le libellé des frais de livraison

**Prochaines actions**
1. Confirmer BUG-1 avec un second email, reproduire BUG-4 et BUG-6 en navigation privée
2. Terminer l'exécution dans Xray (« Exécution 1 »)
3. Réaliser 2 à 3 sessions exploratoires chartées
4. Finaliser le bilan Go/No-Go ([bilan-recette.md](bilan-recette.md))

## 12. Écarts par rapport au plan
| Prévu | Constaté | Conséquence |
|---|---|---|
| Compte de test créé avant l'exécution (critère d'entrée) | L'inscription échoue (BUG-1) | Critère d'entrée non respecté. Les cas de connexion et de déconnexion sont bloqués |
| Passe mobile émulée (vue DevTools) | Non réalisée dans la campagne 1 | La compatibilité est limitée à Chrome desktop. Limite déclarée dans le bilan. Passe à planifier dans une campagne ultérieure |
| Import des cas dans Xray avec le résultat attendu sur la dernière étape | Premier import : résultat attendu décalé sur la première étape, et résultat de TC-CAT-03 tronqué | Fichiers d'import corrigés dans [xray/](xray/), non utilisés pour la campagne en cours. Le défaut est documenté dans le README |
| Critère de sortie sur 100 % des cas | Cas Blocked rendant ce seuil inatteignable | Dénominateur précisé en v0.3 (section 6) |

## 13. Historique des changements
| Version | Date | Changements |
|---|---|---|
| v0.1 | 05/11/2026 | Plan initial : périmètre, approche, critères, risques, planning |
| v0.2 | 05/11/2026 | Constats de la phase de découverte, ajustement des cas de test |
| v0.3 | 06/11/2026 | Précision du critère de sortie (cas Blocked), ajout de l'origine des anomalies, section des écarts, planning avec réalisation, correction du nom du fichier des cas de test (`cas-de-test.csv`), statut mis à jour après l'import dans Xray |
