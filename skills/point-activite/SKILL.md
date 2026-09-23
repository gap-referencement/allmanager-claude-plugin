---
name: point-activite
description: Point d'activité de l'espace AllManager sur une période (mois en cours par défaut) — commandes, devis, SAV, poses, anomalies.
argument-hint: "[période, ex. 'septembre', 'semaine dernière', '2026-09-01 2026-09-30']"
disable-model-invocation: true
allowed-tools: mcp__allmanager
---

Fais un point d'activité de l'espace AllManager.

Période demandée : `$ARGUMENTS` (vide = le mois en cours, du 1er à aujourd'hui).

1. Convertis la période en bornes `AAAA-MM-JJ` et annonce-les.
2. Appelle `mcp__allmanager__get_activity_overview` avec ces bornes.
3. Complète avec `mcp__allmanager__get_anomaly_counters` pour la vue anomalies.
4. Si la période couvre plusieurs semaines, ajoute la répartition hebdomadaire des commandes via
   `mcp__allmanager__count_records` (`resource: sales_order`, `groupBy: week`).

Restitue en français, en langage métier, sous forme de brief :

- **Commandes** : nombre, montant, nouvelles vs clôturées.
- **Devis** : envoyés, signés, perdus.
- **SAV** : ouverts, clos.
- **Poses** : réalisées, restant à planifier.
- **Anomalies** : les compteurs qui méritent attention (retards, paiements, réceptions).
- **À surveiller** : 3 points maximum, concrets.

Pas de nom d'outil, pas de code interne, pas d'identifiant.
