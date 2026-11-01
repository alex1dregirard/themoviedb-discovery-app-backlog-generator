# Générateur de backlog TMDB

Ce dépôt contient le générateur de backlog pédagogique pour les projets
[TMDB des étudiants de BUT Science des données](https://github.com/but-sd/themoviedb-discovery-app-demo-2).
Il est indépendant de l'application : il crée les issues et configure un
GitHub Project dans le dépôt cible explicitement choisi.

## Installation et utilisation

Cloner ce dépôt, puis exécuter les commandes depuis son répertoire :

```sh
git clone https://github.com/but-sd/themoviedb-discovery-app-backlog-generator.git
cd themoviedb-discovery-app-backlog-generator
npm ci
npm run backlog:instantiate -- --repo MON_LOGIN/MON_DEPOT --owner MON_LOGIN --dry-run
npm run backlog:instantiate -- --repo MON_LOGIN/MON_DEPOT --owner MON_LOGIN --apply
```

Node.js 24 ou supérieur et le CLI GitHub `gh` authentifié avec les droits sur
les issues et Projects sont requis. Ne pas cibler ce dépôt de générateur pour
instancier les tickets de l'application.

## Configuration et reprise

Le [backlog YAML](.github/backlog/backlog.yaml) contient 15 tickets Feature,
leurs critères, dépendances, tailles de T-shirt et le milestone **1.1.0**.
Le [guide complet](.github/backlog/README.md) explique l'atelier agile,
les droits GitHub, les vues, les milestones et les clés stables.
Les liens vers les PR de référence restent ceux de l'application.

Pour reprendre le même projet, conserver son numéro :

```sh
npm run backlog:instantiate -- --repo MON_LOGIN/MON_DEPOT --owner MON_LOGIN --project-number NUMERO --apply
```

Sans numéro, `--apply` crée un nouveau projet. Les relances ne dupliquent pas
les issues et conservent leur avancement. L'option `--update-existing` remplace
explicitement leurs titres et descriptions, y compris les modifications
manuelles, mais conserve les commentaires et champs déjà renseignés.
`--config chemin/backlog.yaml` permet de fournir une autre configuration.
La simulation est locale et n'effectue aucun appel GitHub.

L'extraction depuis l'application ne déplace ni ne supprime les issues et
Projects existants. Pour mettre à jour le projet pédagogique de référence :

```sh
npm run backlog:instantiate -- --repo but-sd/themoviedb-discovery-app-demo-2 --owner but-sd --project-number 8 --update-existing --apply
```

## Développement

```sh
npm test
npm run lint
npm run fmt:check
```

Les tests utilisent un CLI GitHub simulé et n'écrivent rien sur GitHub.
La CI exécute les mêmes vérifications.
