---
name: anomalies
description: Anomalies en cours dans AllManager (paiements, ARC, réceptions, retards) — compteurs puis dossiers concernés.
argument-hint: "[commandes | sav | réceptions] [filtre libre]"
disable-model-invocation: true
allowed-tools: mcp__allmanager
---

Anomalies AllManager : `$ARGUMENTS`.

1. Commence par `mcp__allmanager__get_anomaly_counters` pour la vue d'ensemble.
2. Puis `mcp__allmanager__search_anomalies` sur le type de dossier demandé (commandes clients, SAV,
   lignes de commande, réceptions) ; sans précision, commence par les commandes clients.
3. Regroupe par type d'anomalie, les plus nombreuses d'abord ; dans chaque groupe, les dossiers les plus
   anciens d'abord.

Restitue :

- **Vue d'ensemble** : un chiffre par famille d'anomalie.
- **Dossiers** : référence, client, type d'anomalie, depuis quand, en tableau (liste partielle si tronquée).
- **Priorités** : 3 actions concrètes au plus (relancer un paiement, réclamer un ARC, contrôler une réception…).

Pas de nom d'outil, pas de code interne, pas d'identifiant.
