---
name: a-planifier
description: Liste les poses et interventions AllManager qui restent à planifier, et propose des créneaux pour l'une d'elles à la demande.
argument-hint: "[poseur] [--creneaux <référence>]"
disable-model-invocation: true
allowed-tools: mcp__allmanager
---

Reste à planifier dans AllManager : `$ARGUMENTS`.

1. Si un poseur est nommé, résous-le avec `mcp__allmanager__list_technicians` (demande en cas d'homonymes).
2. Appelle `mcp__allmanager__list_interventions_to_plan` (avec le poseur si résolu).
3. Classe le résultat par urgence puis par date souhaitée. Signale les dossiers urgents en premier.
4. Si l'utilisateur demande des créneaux pour une intervention (`--creneaux` ou une référence citée) :
   - retrouve le dossier parent (commande client ou SAV) avec `mcp__allmanager__search_orders` /
     `mcp__allmanager__search_after_sales` ;
   - demande la durée prévue et le nombre de poseurs s'ils ne sont pas connus ;
   - appelle `mcp__allmanager__find_available_slots` ; si la réponse contient une raison sans créneau,
     transmets-la telle quelle en langage clair.

Restitue en tableau : référence, client, chantier, type, durée prévue, date souhaitée, urgent. Puis les
créneaux proposés (jour, heure, poseurs) le cas échéant. Rappelle que la planification elle-même se
fait dans AllManager : ici on lit, on ne modifie pas.

Pas de nom d'outil, pas de code interne, pas d'identifiant.
