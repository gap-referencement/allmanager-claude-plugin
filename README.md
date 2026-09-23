# AllManager pour Claude

Plugin [Claude Code](https://code.claude.com) qui connecte Claude à **AllManager**, l'ERP des
menuisiers et poseurs. Il donne un accès **en lecture seule** aux commandes clients, devis, SAV,
planning, anomalies et rentabilité de **votre espace**, et ajoute des commandes métier prêtes à l'emploi.

Le plugin ne modifie jamais rien dans AllManager.

## Installation

Dans Claude Code :

```
/plugin marketplace add gap-referencement/allmanager-claude-plugin
/plugin install allmanager@allmanager
```

Puis connectez votre compte AllManager :

```
/mcp
```

Choisissez `allmanager` → *Authenticate*. Votre navigateur s'ouvre sur AllManager : connectez-vous,
choisissez l'espace à exposer, validez. Claude Code garde la connexion (jeton renouvelé
automatiquement) ; révoquez-la à tout moment depuis AllManager, *Gestion de l'IA → Connexions MCP*.

En ligne de commande, l'équivalent :

```bash
claude plugin install allmanager@allmanager --scope user
claude mcp login allmanager
```

## Ce que vous pouvez demander

Une fois connecté, posez vos questions en français, par exemple :

- « Où en est la commande Dupont ? »
- « Combien de SAV ouverts ce mois-ci, par poseur ? »
- « Quelles poses restent à planifier pour Julien ? »
- « Propose-moi des créneaux pour la pose Martin, 2 poseurs, une journée. »
- « Quelle est la marge réelle de nos commandes de septembre ? »

Claude répond en langage métier, sur le seul espace autorisé par votre connexion.

### Commandes

| Commande | Ce qu'elle fait |
| --- | --- |
| `/allmanager:point-activite [période]` | Brief d'activité : commandes, devis, SAV, poses, anomalies |
| `/allmanager:planning <personne\|équipe\|tous> [période]` | Planning jour par jour |
| `/allmanager:a-planifier [poseur] [--creneaux <réf>]` | Poses restant à planifier, créneaux proposés |
| `/allmanager:anomalies [commandes\|sav\|réceptions]` | Compteurs puis dossiers en anomalie |
| `/allmanager:dossier <référence\|client\|chantier>` | Fiche d'une commande, d'un devis ou d'un SAV |
| `/allmanager:rentabilite [période] [--par poseur\|équipe]` | Marges prévues vs réelles |

## Prérequis côté AllManager

- Un compte AllManager sur un espace où la fonctionnalité **Assistant IA** est activée.
- Les droits habituels de votre profil s'appliquent : Claude ne voit que ce que vous voyez dans AllManager.

## Pour les administrateurs

- Serveur MCP : `https://api.allmanager.fr/mcp` (HTTP streamable, OAuth 2.1 avec PKCE et
  enregistrement dynamique des clients, scopes `allmanager:read` et `offline_access`).
- Les connexions actives (jetons) se listent et se révoquent dans AllManager, *Gestion de l'IA*.
- Pour viser un autre environnement, définissez `ALLMANAGER_MCP_URL` avant de lancer Claude Code :

  ```bash
  ALLMANAGER_MCP_URL=https://api.preprod.allmanager.fr/mcp claude
  ```

## Développement

```bash
git clone git@github.com:gap-referencement/allmanager-claude-plugin.git
claude plugin validate ./allmanager-claude-plugin
claude --plugin-dir ./allmanager-claude-plugin
```

Les skills (`skills/*/SKILL.md`) se rechargent à chaud ; `.mcp.json` demande `/reload-plugins`.

## Licence

MIT.
