# Générateur de backlog TMDB

Ce dépôt contient le générateur de backlog pédagogique pour les projets
[TMDB des étudiants de BUT Science des données](https://github.com/but-sd/themoviedb-discovery-app-demo-2).
Il est indépendant de l'application : il crée les issues et configure un
GitHub Project dans le dépôt cible explicitement choisi.

## Utiliser le projet avec GitHub Codespaces

Pour travailler sur votre propre copie, ouvrez la page GitHub de ce dépôt et
cliquez sur **Fork**. Une fois le fork créé dans votre compte ou votre
organisation, ouvrez-le, puis choisissez **Code > Codespaces > Create codespace
on [branche]**. Le Codespace s'ouvre directement sur votre fork.

Dans le terminal du Codespace, vérifiez Node.js et installez les dépendances :

```sh
nvm install 24
nvm use 24
npm ci
```

Le projet requiert Node.js 24 ou supérieur (voir `.nvmrc`). Vérifiez également
que le CLI GitHub est connecté :

```sh
gh auth status
```

Le jeton doit permettre de créer des issues et un GitHub Project. Si votre
authentification le permet, ajoutez l'accès aux Projects avec
`gh auth refresh -s project`. Les droits du Codespace et les règles de
l'organisation cible peuvent limiter ces opérations.

Le fork sert de copie du générateur : `--repo` doit désigner le dépôt dans
lequel vous voulez créer les issues et le GitHub Project, pas le fork du
générateur (sauf si c'est réellement votre cible). Depuis le terminal du
Codespace, remplacez `MON_LOGIN` et `MON_DEPOT`, lancez d'abord une simulation,
puis l'import :

```sh
npm run backlog:instantiate -- --repo MON_LOGIN/MON_DEPOT --owner MON_LOGIN --dry-run
npm run backlog:instantiate -- --repo MON_LOGIN/MON_DEPOT --owner MON_LOGIN --apply
```

`--dry-run` ne contacte pas GitHub. Vérifiez soigneusement le dépôt indiqué
avant d'utiliser `--apply`, qui crée réellement les issues et le projet.
Pour reprendre un projet existant, ajoutez `--project-number NUMERO` comme
expliqué ci-dessous.

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
