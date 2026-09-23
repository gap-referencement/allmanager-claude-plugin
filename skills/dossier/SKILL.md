---
name: dossier
description: Retrouve un dossier AllManager (commande client, devis ou SAV) par référence, client ou chantier, et en donne le détail — état, étapes, interventions, anomalies.
argument-hint: "<référence | client | chantier>"
disable-model-invocation: true
allowed-tools: mcp__allmanager
---

Recherche du dossier AllManager : `$ARGUMENTS`.

1. Lance en parallèle `mcp__allmanager__search_orders`, `mcp__allmanager__search_quotes` et
   `mcp__allmanager__search_after_sales` avec la même recherche libre, `limit: 5`.
2. Un seul résultat au total → ouvre-le (`get_order`, `get_quote` ou `get_after_sales`).
   Plusieurs → liste-les (type, référence, client, état, date) et demande lequel ouvrir.
   Aucun → dis-le et propose une recherche plus large (nom partiel, autre orthographe).
3. Pour une commande client, complète avec `mcp__allmanager__search_interventions` (ses poses et
   métrages) et, si la marge est demandée, `mcp__allmanager__get_order_rentability`.

Restitue une fiche : client, chantier, état actuel en clair, étapes franchies et prochaine étape,
montants, interventions (date, poseur, état), anomalies éventuelles, devis ou SAV liés.

Pas de nom d'outil, pas de code interne, pas d'identifiant.
