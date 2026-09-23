---
name: rentabilite
description: Rentabilité AllManager — marge de l'espace sur une période, marge par commande, heures et chantiers par poseur ou équipe.
argument-hint: "[période] [--par poseur|équipe] [--commande <référence>]"
disable-model-invocation: true
allowed-tools: mcp__allmanager
---

Rentabilité AllManager : `$ARGUMENTS`.

1. Convertis la période en bornes `AAAA-MM-JJ` (défaut : le mois en cours) et annonce-les.
2. Selon la demande :
   - vue d'ensemble → `mcp__allmanager__get_rentability_overview` ;
   - une commande → `mcp__allmanager__search_orders` puis `mcp__allmanager__get_order_rentability` ;
   - classement des commandes (meilleure / pire marge) → `mcp__allmanager__search_order_rentabilities`
     avec `sortBy` + `sortDescending` + `limit` (un seul appel, pas de parcours de pages) ;
   - par poseur ou par équipe → `mcp__allmanager__list_workforce_rentability` avec `groupBy`.
3. Distingue toujours **prévu** et **réel** (marge, coûts), en euros et en pourcentage.

Restitue en tableau, puis 2 ou 3 constats : écarts prévu/réel notables, commandes à marge négative,
poseurs ou équipes en dessous de la moyenne. Ne commente pas les personnes au-delà des chiffres.

Pas de nom d'outil, pas de code interne, pas d'identifiant.
