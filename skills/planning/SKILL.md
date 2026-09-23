---
name: planning
description: Planning d'un poseur, d'une équipe ou de tout l'espace AllManager sur une période.
argument-hint: "<personne | équipe | 'tous'> [période, ex. 'semaine prochaine']"
disable-model-invocation: true
allowed-tools: mcp__allmanager
---

Affiche le planning AllManager demandé : `$ARGUMENTS`.

1. Sépare le **qui** (une personne, une équipe ou « tous ») du **quand** (défaut : la semaine en cours, lundi → dimanche).
2. Résous le qui :
   - personne → `mcp__allmanager__list_technicians` avec `query` ; plusieurs résultats → demande lequel avant de continuer ;
   - équipe → `mcp__allmanager__list_teams` ;
   - « tous » ou rien → pas de filtre (planning de l'espace).
3. Appelle `mcp__allmanager__get_planning` avec les identifiants résolus et les bornes `AAAA-MM-JJ`.
4. Si le planning est vide, dis-le et propose de regarder ce qui reste à planifier (`mcp__allmanager__list_interventions_to_plan`).

Restitue jour par jour, puis par personne : heure de début → fin, type (pose, métrage, SAV, livraison,
enlèvement, absence…), client et chantier. Termine par les trous notables (demi-journées libres) si la
question porte sur la disponibilité.

Pas de nom d'outil, pas de code interne, pas d'identifiant.
