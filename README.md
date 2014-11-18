# Task List &nbsp; [![Build Status](https://travis-ci.org/codurance/task-list.png)](https://travis-ci.org/codurance/task-list)

Voici un exemple de code obsédé par les primitives 

Une *primitive* est un concept technique par nature qui n'est pas pertinent pour votre domaine métier. Cela inclus les entiers, caractères, les chaines de caractères et les collections (listes, sets, maps, etc...), mais aussi des choses comme les threads, readers, writers, parsers, exceptions et tout ce qui est purement focalisé sur des points techniques. De l'autre coté, les concepts métiers de ce projet, les taches (task), les projets (project), etc... devraient faire partie de votre *domaine métier*. Le domaine métier est le langage du métier avec lequel vous travaillez, et le fait de l'utiliser dans votre code permet d'éviter de parler un langage différent et aide à éviter les incompréhensions. Dans notre expérience, les incompréhensions de ce genre sont une des plus grosses causes de bugs.

#Exercice

Essayer d'implémenter les évolutions suivantes, en refactorant les primitives au fur et à mesure. Essayer de ne pas implémenter un nouveau comportement tant que le code que vous souhaiter faire évoluer n'a pas étant entièrement refactoré pour supprimer les primitives.

Un ensemble de critère pour identifier si les primitives ont été supprimées est de ne permettre les primitives uniquement dans les paramètres des constructeurs, en temps que variables locales et dans des variables privées. Les primitives ne doivent pas être passées en paramètre ni retourné par une fonction. La seule exception sera du code d'infrastructure (code qui communique avec le terminal, le réseau, la base de données, ...) L'infrastructure nécessite la sérialisation des primitives mais doit être traiter comme un cas à part. Vous pouvez considérer votre infrastructure comme un domaine à part, domaine technique par nature, dans lequel les primitives *sont* le domaine.

Vous pouvez essayer d'englober les tests autour du comportement que vous êtes en train de refactorer. Au début ce sera surtout des tests de haut niveau, mais n'hésitez pas à écrire de nouveaux tests unitaires au fur et à mesure de l'implémentation des nouvelles fonctionnalités.


### Fonctionnalités:

 1. Date limite (deadline)
    1. On peut donner à chaque tâche une date limite optionnelle avec la commande `deadline <ID> <date>`
    2. Afficher l'ensemble des tâches à faire aujourd'hui avec la commande `today`
  2. IDs customisable
    1. Permettre aux utilisateurs de spécifier un identifiant qui n'est pas un nombre.
    2. Rejeter les espaces et les caractères spéciaux de cet ID.
  3. Suppression
    1. Permetttre aux utilisateurs de supprimer des taches avec la commande `delete <ID>`.
  4. Affichage
    1. Affichage des taches par date, avec la commande `view by date`.
    2. Affichage des tâches par date limite, avec la commande `view by deadline`.
    3. Ne supprimer pas la fonctionnalité qui permet aux utilisateurs de voir les tâches par projet, mais modifier la commande en : `view by project`.

### Réflexions et Approches (Démarches)

Penser à *l'attraction/succès/attrait/attirance du comportement*. Assez souvent, vous pouvez réduire le nombre de comportement, du monde extérieur, qui repose sur les primitives (opposées aux primitives internes stockées comme champs privés ou variables locales) simplement en déplaçant ce comportement dans un *objet metier* qui contient ces primitives. Si vous n'avez pas d'objet métier metier, créez-en un. Ces objets métiers sont communément appelés *attracteur de comportement* (*aimant à comportement*?), parce que lorsqu'ils sont créé, ils rendent évident le lieu où le comportement de ces objets doit être.

Un principe associé est de considérer le type de l'objet que vous êtes en train de créer. S'agit-il d'un objet "collection" (ou *enregistrement* / *archive*), qui consiste simplement en un ensemble de methodes `getFoo` qui retourne leurs primitives internes (à n'utiliser qu'avec des infrastructures, évidemment), ou bien s'agit-il d'un objet avec un comportement ? S'il s'agit de ce dernier, vous ne devez exposer aucun de ses états internes. Un enregistrement, de son coté, ne doit contenir aucun comportement. Traiter quelque chose comme un enregistrement et un objet métier ne donne généralement rien de bon.

Votre approche devra dépendre de tout ce que vous aurez appris concernant les approches fonctionnelles et orientées-objets pour modéliser votre domaine. Ces deux approches encourage l'encapsulation, mais la technique de *masquer les informations* est généralement utilisé seulement dans les langages orientés-objets. Elles diffèrent aussi dans l'approche utilisé pour extraire le comportement; la programmation fonctionnelle fonctionne souvent avec des ensembles fermés de comportement à travers *tagged unions*, de son coté en programmation orientée-objet, nous utilisons le *polymorphisme* pour arriver à nos fins de façon ouverte et extensible.

Séparez vos demandes et vos requêtes (commands & queries). Dites à un objet de faire quelque chose, ou demander lui de vous retourner une information, mais ne faites pas les deux.


Enfin, pensez aux principes SOLID lorsque vous refactorez:
  * Prévoyez de casser les gros blocs de comportements en petits, chaque fragments ayant une responsabilité unique.
  * Pensez au formats dans lequel il devrait être simple d'étendre l'application.
  * Ne surprennez pas ceux qui vous appellent. Conformez-vous aux interfaces.
  * Séparez les comportements en se basant sur les besoins.
  * Dépendez des abstractions.
  
