# Un backlog agile pour les étudiants

Le fichier [`backlog.yaml`](backlog.yaml) contient une **epic active et 15 tickets
Feature** pour construire un tableau de bord d'exploration des films sur plusieurs
années. Les anciennes idées restent commentées et ne sont pas importées.
Chaque étudiant peut l'adapter, puis instancier les
issues et un GitHub Project dans **son propre dépôt personnel ou d'organisation**. Rien n'est créé dans le
dépôt de l'enseignant automatiquement.

## Contenu de la configuration

Chaque évolution contient un identifiant stable, un objectif pédagogique et des
tickets. Chaque ticket possède une user story, une estimation, une priorité, des
critères d'acceptation et des dépendances. Par défaut, `id` sert aussi à
reconnaître les issues lors d'une reprise. Pour renuméroter un ticket déjà
importé, conserver sa valeur `key` ou, si elle n'existe pas encore, placer son
ancien identifiant dans `key` avant de modifier `id`.
La clé stable, unique dans le backlog, reste ensuite inchangée ; `dependsOn`
utilise toujours les identifiants affichés actuels.

Chaque ticket porte `"type": "feature"` ou `"type": "bug"`. Dans une organisation, le générateur
utilise les **types d'issue natifs GitHub Feature et Bug** et vérifie qu'ils sont activés.
Dans un dépôt personnel, il utilise automatiquement un **label feature ou bug**, créé
s'il manque. Ce choix est annoncé pendant l'import. Une erreur d'accès à l'API
ou un type requis désactivé dans une organisation reste une erreur explicite,
pas un basculement silencieux vers un label.

### Epic active : exploration par année de sortie

L'objectif est de comparer les films selon leur **année de sortie** sur une
période configurable, par exemple 2015 à 2024. Il ne s'agit pas de reconstituer
l'évolution historique des notes ou de la popularité : ces valeurs sont celles
observées à la collecte. Une collecte plafonnée reste un échantillon, pas un
recensement exhaustif.

| Ticket       | Livraison attendue                                 | Taille | Prérequis                  |
| ------------ | -------------------------------------------------- | ------ | -------------------------- |
| DASHBOARD-01 | Spécification fournie des données et relations     | S      | Aucun                      |
| DASHBOARD-02 | Mise en place du schéma SQLite spécifié            | M      | DASHBOARD-01               |
| DASHBOARD-03 | Base locale et migrations                          | M      | DASHBOARD-02               |
| DASHBOARD-04 | CLI d'initialisation et chemin configurable        | S      | DASHBOARD-03               |
| DASHBOARD-05 | Import CSV et entretien des genres                 | M      | DASHBOARD-04               |
| DASHBOARD-06 | Import CSV et entretien des pays de production     | M      | DASHBOARD-05               |
| DASHBOARD-07 | Route Discover paginée par année                   | M      | DASHBOARD-01               |
| DASHBOARD-08 | Extraction TMDB par année et pagination            | L      | DASHBOARD-01, DASHBOARD-07 |
| DASHBOARD-09 | Nettoyage et contrôles de qualité                  | M      | DASHBOARD-01, DASHBOARD-08 |
| DASHBOARD-10 | Chargement idempotent et suivi des exécutions      | L      | DASHBOARD-03, DASHBOARD-09 |
| DASHBOARD-11 | CLI de pilotage et simulation                      | M      | DASHBOARD-04, DASHBOARD-10 |
| DASHBOARD-12 | API de lecture et agrégations                      | L      | DASHBOARD-10               |
| DASHBOARD-13 | Page, filtres et tableau paginé                    | M      | DASHBOARD-12               |
| DASHBOARD-14 | Visualisations et interprétation                   | L      | DASHBOARD-13               |
| DASHBOARD-15 | Démonstration reproductible et tests d'intégration | M      | DASHBOARD-11, DASHBOARD-14 |

DASHBOARD-04, DASHBOARD-05 et DASHBOARD-06 spécifient respectivement les apports des PR
[#50](https://github.com/but-sd/themoviedb-discovery-app-demo-2/pull/50),
[#51](https://github.com/but-sd/themoviedb-discovery-app-demo-2/pull/51) et
[#52](https://github.com/but-sd/themoviedb-discovery-app-demo-2/pull/52).
Ces PR contiennent aussi le socle des précédentes : les tickets distinguent
leurs apports pour éviter de redemander les mêmes développements.
DASHBOARD-07 décrit la route Discover de la
[#56](https://github.com/but-sd/themoviedb-discovery-app-demo-2/pull/56) :
une seule page pour une année de sortie principale, sans collecte automatique,
enrichissement par le détail ni stockage.
Les anciens EXPL-04 à EXPL-11 correspondent à DASHBOARD-08 à DASHBOARD-15 ; leurs clés stables
conservent leurs issues et leur suivi. Les nouveaux tickets ont des clés
distinctes pour ne pas reprendre les anciennes issues portant ces numéros.
Relancer avec `--update-existing --apply` pour actualiser les titres et les
dépendances des issues déjà importées.

L'epic EXPL est renommée DASHBOARD. Les identifiants affichés et les dépendances
utilisent DASHBOARD ; les clés stables `EXPL-*` restent inchangées pour retrouver
les mêmes issues et conserver leur historique. Dans un projet existant, renommer
l'option EXPL du champ Epic en DASHBOARD avant l'import conserve les rattachements.

DASHBOARD-01 décrit le besoin d'exploration, le parcours Discover puis détail d'un film
et un dictionnaire des données brutes avec leur provenance dans l'API TMDB.
Les liens entre films, genres et pays sont expliqués par des règles métier et
des exemples, sans fournir les entités ni le MCD. Les étudiants en déduisent
eux-mêmes le MCD dans DASHBOARD-02, puis le traduisent en schéma SQLite.
Le suivi détaillé des exécutions des étapes suivantes reste une extension distincte.

Dans DASHBOARD-01, l'année de sortie est une information dérivée de la date de sortie,
et non l'année demandée au collecteur. Une date absente ou inexploitable laisse
l'année inconnue ; le film reste au catalogue mais ne participe pas aux
regroupements par année. DASHBOARD-02 choisit de traduire cette règle par une colonne
SQLite générée et stockée, avec les tests du calcul, de sa mise à jour automatique
et de l'interdiction d'écrire directement cette colonne.

DASHBOARD-03 et DASHBOARD-07 peuvent avancer en parallèle après leurs prérequis.
Chaque ticket possède ses critères d'acceptation ; le
champ Epic du projet associe les tickets à cette évolution, sans créer d'issue
parent supplémentaire. Ce backlog décrit le travail futur, il n'implémente pas
encore la base, le CLI ni le tableau de bord.

Chaque ticket définit sa propre **Definition of Done** dans la liste obligatoire
`definitionOfDone` du YAML. Elle décrit les vérifications et livrables nécessaires
pour terminer ce ticket, distincts de ses critères d'acceptation. Le générateur
la rend sous forme de cases à cocher, sans liste globale ni valeur par défaut.

La configuration `project.typeViews` définit deux vues supplémentaires :
**Features** et **Bugs**. Elles sont filtrées respectivement avec `type:Feature`
et `type:Bug` en organisation, ou `label:feature` et `label:bug` sur un dépôt
personnel. **Backlog** reste la vue globale ; **Epics** fournit une vue dédiée
aux tickets associés à chaque epic. Les relances ne dupliquent pas ces vues ;
les filtres de **Features** et **Bugs** sont synchronisés avec la configuration.
Title, Epic et Size sont visibles dans Backlog, Features et Bugs ; Title et Epic
sont visibles dans Epics.

### Estimer en tailles de T-shirt

| Taille | Repère pour la discussion                                        |
| ------ | ---------------------------------------------------------------- |
| XS     | Changement très local, peu d'incertitude                         |
| S      | Petite fonctionnalité, périmètre clair                           |
| M      | Plusieurs composants ou transformations à coordonner             |
| L      | Fonctionnalité plus large, intégration ou incertitude importante |
| XL     | Trop large ou trop incertain : découper avant le sprint          |

Ces tailles évaluent **l'effort relatif, la complexité et l'incertitude**, pas des
heures. Elles ne servent ni à noter les étudiants ni à comparer les équipes. Les
estimations proposées sont un point de départ à **réestimer collectivement**.
Après une réestimation dans GitHub, ajuster uniquement le champ Size.
Le générateur ne crée ni ne renseigne de champ numérique Weight ; un ancien
champ Weight peut être supprimé manuellement dans les paramètres du projet.

La propriété `color` de chaque taille définit sa couleur GitHub : **XS vert,
S bleu, M jaune, L orange, XL rouge**. Le générateur applique cette palette à
chaque création et relance, sans changer les identifiants des options ni les
estimations des tickets. Les couleurs personnalisées de ces cinq tailles sont
donc remplacées par celles de la configuration ; les options supplémentaires
et les descriptions existantes sont conservées.

P1 = socle conseillé ; P2 = enrichissement ; P3 = approfondissement. Une priorité
ne remplace pas les dépendances : commencer par les tickets dont les prérequis
sont satisfaits. Le backlog complet n'est pas un engagement à tout réaliser.

## Instancier dans son dépôt

Exécuter les commandes depuis le clone de
`but-sd/themoviedb-discovery-app-backlog-generator`, et non depuis le dépôt de
l'application. `--repo` désigne le dépôt étudiant ou le dépôt de l'application
à alimenter ; les chemins de configuration sont ceux du générateur.

Prérequis :

- Un dépôt GitHub personnel ou d'organisation avec les issues activées et un
  accès en écriture. Pour une organisation, les types Feature et Bug doivent être activés ;
  pour un dépôt personnel, les labels feature et bug sont gérés automatiquement.
- Node.js dans la version prévue par le projet et le CLI `gh` à jour, disponible
  sur le PATH. Exécuter `npm ci` pour installer les dépendances, dont le lecteur YAML.
- Une authentification GitHub autorisant les issues et les Projects. Pour une
  connexion OAuth classique au CLI :

```sh
gh auth login
gh auth refresh -s project
```

Avec un autre type de jeton, vérifier les droits correspondants ; les règles
d'une organisation peuvent restreindre la création de projets. Ne jamais écrire
un jeton dans la configuration.

Depuis la racine de la copie du projet, remplacer `MON_DEPOT` et, pour une
cible d'organisation, `MON_ORGANISATION`. Sans `--owner`, le générateur utilise
le login authentifié par `gh`, ce qui convient à un dépôt personnel. Pour une
organisation, fournir son nom avec `--owner`. Le dépôt cible et le GitHub
Project doivent appartenir au même propriétaire ; `--repo` accepte uniquement
le nom du dépôt, sans préfixe propriétaire.

```sh
# Vérifier les tickets localement : aucune requête GitHub, aucune écriture.
npm run backlog:instantiate -- --repo MON_DEPOT --owner MON_ORGANISATION --dry-run

# Créer réellement un projet et les issues dans le dépôt indiqué explicitement.
npm run backlog:instantiate -- --repo MON_DEPOT --owner MON_ORGANISATION --apply
```

Le script crée un GitHub Project lié au dépôt, les issues et les champs :
**Status**, **Size**, **Epic** et **Priority**. Tous les nouveaux
tickets sont initialisés dans **Backlog**. Le champ Status propose ensuite
**Ready**, **In Progress**, **In Review** et **Done**. Les dépendances sont des
liens dans les descriptions des issues, pas des blocages natifs GitHub.
Chaque nouvelle issue reçoit le type natif **Feature ou Bug** (organisation) ou le label
**feature ou bug** (dépôt personnel). Lors d'une relance, les issues existantes reçoivent
également cette classification si nécessaire, sans modifier leurs autres
propriétés ni retirer leurs labels existants. Cette attribution ne nécessite pas
`--update-existing`, qui ne concerne que les titres et descriptions.

Le script garantit une vue **Backlog**, nommée par `project.viewName` dans la
configuration. La vue initiale unique « View 1 » est renommée sans changer sa
mise en page. Si des vues personnalisées existent déjà, elles sont conservées et
une vue tabulaire Backlog est ajoutée si nécessaire. Les relances ne créent pas
de vue en double. Les colonnes **Epic** et **Size** sont rendues visibles
immédiatement après **Title**, dans cet ordre, sans retirer les autres colonnes
ni modifier leur ordre relatif.

La vue tabulaire **Epics**, nommée par `project.epicViewName`, affiche **Title**
et **Epic**. Pour regrouper les tickets dans GitHub, choisir **Group by > Epic**
dans le menu de la vue. Les lignes restent des tickets : le générateur ne crée
pas d'issues parent pour représenter les epics. Le regroupement est une
préférence de vue à sélectionner dans GitHub.

Pour utiliser un tableau Kanban, choisir la disposition **Board** de la vue
Backlog dans GitHub, la grouper par **Status**, puis afficher Size, Epic et
Priority. Le script ne modifie pas les filtres ou regroupements ; il ajoute
uniquement Epic et Size aux colonnes visibles si nécessaire. Dans la vue
**Epics**, choisir **Group by > Epic** pour voir les tickets par epic.

### Reprendre un import ou utiliser un projet existant

La création affiche immédiatement l'URL et le numéro du projet, ainsi que la
commande de reprise. Conserver ce numéro :

```sh
npm run backlog:instantiate -- --repo MON_DEPOT --owner MON_ORGANISATION --project-number 7 --apply
```

**Sans `--project-number`, chaque lancement avec `--apply` crée un nouveau
projet.** Avec le numéro, les issues sont retrouvées par leur marqueur stable,
y compris celles qui sont fermées. Les issues existantes ne sont ni réouvertes
ni réécrites. Les champs déjà renseignés, notamment les estimations revues et le
statut d'avancement, sont conservés ; seuls les champs manquants sont initialisés.

Pour un projet préexistant créé autrement, ajouter d'abord l'option **Backlog**
au champ natif **Status**. Les champs personnalisés existants doivent avoir les
types attendus ; sinon le script s'arrête avec une erreur explicite. Les options
manquantes de Size, Epic et Priority sont ajoutées sans retirer les anciennes.
Il ne remplace pas les options de statut d'un projet existant.

L'import n'est pas transactionnel : en cas d'erreur, les créations déjà effectuées
restent sur GitHub. Corriger le problème et reprendre avec le même numéro.
Ne pas lancer deux imports simultanément vers le même dépôt/projet.

Pour modifier le backlog avant import, éditer `backlog.yaml` ou utiliser une
copie avec `--config chemin/mon-backlog.yaml`. Après import, les modifications
de contenu restent à effectuer dans GitHub : le script n'est pas un outil de
synchronisation destructive.

### Planifier des versions avec les milestones

Les milestones GitHub sont définis au niveau du dépôt dans `backlog.yaml` :

```yaml
milestones:
  - title: 1.1.0
    description: Version 1.1.0 de l'application.
```

Le générateur crée les milestones manquants avec `--apply` et réutilise ceux
dont le titre correspond exactement, même fermés. Il ne modifie ni leur
description ni leur état lorsqu'ils existent déjà. Sans cette section, aucun
milestone n'est créé. La simulation affiche les définitions sans appeler GitHub.

Pour affecter un ticket à une version, ajouter `milestone: 1.1.0` à ce ticket,
au même niveau que `size` et `priority`. Le titre doit être déclaré dans
`milestones`. Ce rattachement est appliqué aux nouveaux tickets et aux tickets
existants sans milestone ; un milestone déjà affecté manuellement est conservé,
même avec `--update-existing`. Omettre ou retirer cette propriété ne détache
jamais un ticket de sa version. Les erreurs de permission arrêtent l'import.

La configuration fournie crée **1.1.0** et y rattache les trois premiers tickets,
**DASHBOARD-01**, **DASHBOARD-02** et **DASHBOARD-03**. L'équipe peut ajuster le périmètre des
versions suivantes pendant la planification.
Une milestone regroupe un objectif de livraison, tandis que le statut du projet
décrit l'avancement des tickets.

### Ajouter de nouvelles idées et relancer

Ajouter des tickets à une évolution existante ou ajouter une évolution dans
`backlog.yaml`. Utiliser de **nouveaux identifiants stables** : par exemple
`EXP-04` pour un ticket supplémentaire, ou `NEW` et `NEW-01` pour une nouvelle
évolution. Fournir les mêmes propriétés que les tickets existants, avec une
taille, une priorité, des critères d'acceptation et une liste de dépendances.

Après récupération de la nouvelle configuration, chaque étudiant relance vers
**le même numéro de projet** :

```sh
npm run backlog:instantiate -- --repo MON_DEPOT --owner MON_ORGANISATION --project-number 7 --dry-run
npm run backlog:instantiate -- --repo MON_DEPOT --owner MON_ORGANISATION --project-number 7 --apply
```

La simulation reste locale : elle affiche le backlog configuré, pas la différence
avec GitHub. L'import ajoute uniquement les issues absentes et les rattachements
manquants. Les nouveaux tickets démarrent dans Backlog ; une nouvelle évolution
ajoute automatiquement son option dans le champ Epic, en conservant les
identifiants, couleurs et descriptions des options existantes.

Une nouvelle relance sans modification ne crée aucun doublon. Les tickets déjà
en cours ou terminés, leurs contenus, leurs estimations et les options ajoutées
par l'équipe restent intacts. Sans `--prune`, retirer une idée du fichier ne supprime rien dans
GitHub. Modifier un ticket existant dans le fichier ne remplace pas son contenu
sur GitHub : utiliser un nouvel identifiant pour un nouveau besoin, ou modifier
l'issue manuellement pour préciser le besoin initial, ou utiliser la mise à jour
explicite ci-dessous.

### Nettoyer les tickets retirés du backlog

`--prune` exige un `--project-number` explicite. Il ferme les issues générées
absentes du YAML et les retire du projet ciblé, sans suppression définitive.
Les issues manuelles, les pull requests, les issues hors de ce projet et celles
dont la clé stable existe encore dans le YAML sont conservées. Une issue avec
plusieurs marqueurs est conservée par précaution. Les autres projets ne sont
pas modifiés, mais la fermeture de l'issue reste visible partout.

```sh
npm run backlog:instantiate -- --repo MON_DEPOT --owner MON_ORGANISATION --project-number 7 --prune --dry-run
npm run backlog:instantiate -- --repo MON_DEPOT --owner MON_ORGANISATION --project-number 7 --prune --apply
```

Contrairement à la simulation ordinaire, la simulation avec `--prune` consulte
GitHub pour afficher les issues concernées, sans écriture. Avec `--apply`, le
nettoyage intervient après l'import et les éventuelles mises à jour. Une erreur
arrête le traitement ; les opérations déjà effectuées restent appliquées et
une relance reprend le nettoyage. Les commentaires et l'historique des issues
fermées sont conservés. Vérifier le résultat simulé et les clés stables avant
application : modifier une clé peut faire considérer son ancienne issue comme
obsolète.

### Mettre à jour le contenu des tickets existants

Pour répercuter les changements de titres, user stories, critères d'acceptation,
objectifs pédagogiques ou dépendances, ajouter `--update-existing` :

```sh
npm run backlog:instantiate -- --repo MON_DEPOT --owner MON_ORGANISATION --project-number 7 --update-existing --dry-run
npm run backlog:instantiate -- --repo MON_DEPOT --owner MON_ORGANISATION --project-number 7 --update-existing --apply
```

**Cette option remplace le titre et toute la description de chaque issue dont le
contenu diffère de la configuration**, y compris les modifications manuelles et
les cases cochées dans la description. Prévenir les étudiants avant de l'utiliser.
Les commentaires, affectations, labels, état ouvert/fermé et champs déjà
renseignés du projet restent conservés, notamment l'avancement et Size.
Les estimations affichées dans la description restent les estimations initiales
de la configuration, pas celles réévaluées dans les champs du projet.

Les liens de dépendances sont reconstruits après création des nouveaux tickets.
Les contenus identiques ne sont pas réécrits. Sans cette option, la relance reste
strictement additive pour les titres et descriptions. La simulation sans `--prune` ne consulte
pas GitHub et ne détermine donc pas quelles issues ont changé.

## Proposition d'atelier agile

1. **Refinement** : lire les user stories, discuter les critères d'acceptation,
   repérer les dépendances et comparer les tailles à un ticket de référence.
2. **Estimation collective** : chacun propose une taille, puis les estimations
   extrêmes sont expliquées avant de chercher un accord. Découper le ticket XL
   en petits tickets livrables ; garder l'original comme référence.
3. **Sprint planning** : choisir un objectif, sélectionner un petit ensemble
   réaliste et déplacer seulement les tickets prêts dans Ready. Un ticket prêt
   a des critères compris, une taille discutée et des prérequis satisfaits.
4. **Réalisation** : limiter les travaux simultanés, passer par In Progress puis
   In Review et relier les pull requests aux issues. Chaque équipe peut ajouter
   un champ Sprint pour suivre sa sélection.
5. **Review** : démontrer les critères d'acceptation et appliquer la Definition
   of Done du ticket avant de le passer dans Done.
6. **Rétrospective** : comparer ce qui a été prévu et livré, discuter les
   incertitudes et ajuster le découpage et les estimations du sprint suivant.

Pour une première itération, lire la spécification **DASHBOARD-01**, puis réaliser le
schéma **DASHBOARD-02** et discuter le contrat d'extraction **DASHBOARD-08**. Construire ensuite un petit
incrément de bout en bout sur une période limitée avant d'élargir la collecte.
Les tailles restent à réestimer ; découper les tickets L si nécessaire pour
tenir l'objectif du sprint.

## Vérifier le dispositif

```sh
npm run backlog:test
```

Ces vérifications couvrent la configuration, les dépendances, la simulation,
l'import et la reprise avec un CLI GitHub simulé. Elles ne créent aucune issue et
ne consomment pas de jeton. Le fonctionnement réel dépend des droits GitHub et
des politiques du compte cible.
