---
name: allmanager
description: Mode d'emploi du serveur AllManager (ERP menuiserie / pose). À charger dès qu'une question porte sur des commandes clients, devis, SAV, poses, interventions, planning, techniciens, équipes, anomalies, marges ou rentabilité d'une entreprise utilisant AllManager. Explique comment enchaîner les outils `mcp__allmanager__*`, leurs pièges et le registre de réponse attendu.
when_to_use: 'Exemples : où en est la commande ; combien de SAV ouverts ; planning de Julien la semaine prochaine ; quelles poses restent à planifier ; propose un créneau ; marge de la commande ; anomalies en cours ; point du mois.'
user-invocable: false
---

# AllManager pour Claude

AllManager est l'ERP d'entreprises de menuiserie / pose. Les outils `mcp__allmanager__*` donnent un
accès **en lecture seule** à **un seul espace** (l'entreprise liée au compte connecté). Impossible
de lire ou modifier quoi que ce soit ailleurs : aucun argument ne choisit l'espace.

## Registre de réponse

Tu parles à des menuisiers, poseurs, commerciaux et gestionnaires, pas à des développeurs.

- Réponds en français, en langage métier : « commandes », « devis », « SAV », « poses », « planning », « anomalies ».
- Ne montre jamais un nom d'outil, un code d'état brut (`open`, `intervention_to_plan`…), un UUID, un nom de champ.
  Les outils renvoient déjà des libellés en français : utilise-les.
- Une liste tronquée (`truncated: true`) n'est pas une erreur : dis que la liste est partielle et propose d'affiner.
- Un refus (droit manquant, fonctionnalité non activée) se dit simplement : « cet accès n'est pas ouvert sur votre compte ».
- Cite toujours la période et les filtres utilisés quand tu donnes un chiffre.

## Premier appel

Si tu ne sais pas encore pour qui tu travailles, appelle `whoami` : il donne l'espace courant. Ne le
redemande pas ensuite dans la même conversation.

## Choisir le bon outil

| Besoin | Outil | Notes |
| --- | --- | --- |
| Compter, totaliser, répartir (« combien de… par… ») | `count_records` | **Un seul appel** : `resource` (`sales_order`, `sales_quote`, `after_sales`, `intervention`) + `groupBy` (`salesman`, `technician`, `team`, `client`, `client_type`, `state`, `type`, `day`, `week`, `month`…). `minCount: 2` répond à « quels clients en ont plusieurs ». Ne pagine jamais des listes pour compter. |
| Point d'activité d'une période | `get_activity_overview` | Par défaut : le mois en cours. |
| Retrouver / lister des commandes clients | `search_orders` → `get_order` | Filtres : `query`, `states`, `orderType`, `from`/`to`, `technicianIds`, `clientType`. |
| Devis | `search_quotes` → `get_quote` | États : brouillon, à présenter, envoyé, signé, perdu, reporté… |
| SAV / dépannages | `search_after_sales` → `get_after_sales` | Types : SAV, dépannage. |
| Interventions (poses, métrages, SAV) | `search_interventions` | Filtres par état, type, dates, poseur. |
| Ce qui reste à planifier | `list_interventions_to_plan` | Optionnel : limiter à un poseur. |
| Personnes et équipes | `list_technicians`, `list_teams` | **À résoudre avant** tout planning : `get_planning` attend des identifiants. |
| Planning d'une personne / équipe / de tout l'espace | `get_planning` | `from`/`to` inclus. Sans personne ni équipe : tout l'espace (question de capacité). |
| Proposer des créneaux de pose | `find_available_slots` | Il faut le dossier (`sales_order` ou `after_sales` + son identifiant), la durée en minutes et le nombre de poseurs. Une réponse `reason` sans créneau est une explication à transmettre, pas une panne. |
| Anomalies (retards, paiements, ARC, réceptions…) | `search_anomalies`, `get_anomaly_counters` | Les compteurs donnent la vue d'ensemble ; la recherche donne les dossiers concernés. |
| Marge d'une commande / de l'espace | `search_order_rentabilities`, `get_order_rentability`, `get_rentability_overview` | Marge prévue vs réelle, coûts prévus vs réels. |
| Heures et chantiers par poseur ou équipe | `list_workforce_rentability` | `groupBy: installer` ou `team`. |

## Règles d'efficacité

- **Superlatifs** (« la plus grosse commande », « le SAV le plus ancien », « le devis le plus récent ») :
  un seul appel avec `sortBy` + `sortDescending` + `limit: 1`. Jamais de parcours des pages.
- **Pagination** : les listes s'arrêtent à la page 3. Au-delà, affine les filtres ou compte avec `count_records`.
- **Dates** : format `AAAA-MM-JJ`. « Cette semaine », « le mois dernier » : calcule les bornes à partir de la
  date du jour et annonce-les dans la réponse.
- **Personnes** : une question sur « Julien » ou « l'équipe de Nantes » commence par `list_technicians`
  ou `list_teams` avec `query`, puis utilise l'identifiant obtenu. En cas d'homonymes, demande lequel.
- **Commandes vs fournisseurs** : `sales_order` désigne toujours la commande **client** ; l'approvisionnement
  fournisseur (`in_supplier_order`, `in_goods_receipt`) est une étape de son cycle de vie, pas un dossier à part.
- **Détail** : `get_*` n'accepte que l'identifiant renvoyé par la recherche correspondante. Ne devine jamais un identifiant.

## Restitution

- Une liste courte (≤ 10) : puces `Référence · Client · État · Date` avec ce qui compte pour la question.
- Une liste longue : tableau, puis un total et la mention « liste partielle » si tronquée.
- Un planning : par jour, puis par personne, heure de début → fin, type d'évènement, chantier.
- Un chiffre : le chiffre, sa période, son périmètre, et la comparaison demandée s'il y en a une.
- Termine par la suite logique quand elle s'impose (« voulez-vous le détail de… », « je peux proposer des créneaux »).
