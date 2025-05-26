# SUIVI_STAGE

## Auteur

Yaroslava Khrabrova

## Date


## Période : 28 avril - 2 mai

### Intégration dans l’équipe et les projets

- Découverte de l’environnement technique: prise en main des outils de gestion de code (Mercurial), gestion de projet (Azure DevOps), monitoring (APM), les logiciels spécifique pour l'entreprise Promod.
- Présentation du fonctionnement de l’équipe, des processus de développement, et des projets existants (API, applications legacy).
- Compréhension des architectures en place et des bases de code.


### Tâche: Paramétrage de l’intégration de nouveaux marketplaces

**Contexte**

- Certains marketplaces nécessitent l’impression d’une étiquette de retour à inclure dans le colis, d’autres non.
- Les traductions des factures sont actuellement codées en dur, ce qui complique la maintenance et l’évolution.

**Analyse de l’existant**

- Chaque nouveau paramètre marketplace rend l’évolution difficile et peu maintenable.

**Travail réalisé**

- Modélisation de la base de données, création et adaptation de la table de paramétrage pour intégrer les nouveaux paramètres.
- Utilisation de la technologie de DB Browser, manipulation de la base de données pour ajouter et modifier les paramètres nécessaires.
- Intégration dans le code Java, adaptation du code pour lire dynamiquement les paramètres depuis la base, suppression progressive des conditions dans le code legacy.
- Analyse et adaptation de la méthode pour utiliser le nouveau paramétrage.
- Revue des outils et composants existants pour garantir la cohérence de l’architecture.


### Tâche: Suivi de l’expédition des colis

**Contexte**

- Permettre au magasin de définir un colis comme «expédié» lors de la remise au transporteur, pour assurer le suivi.

**Travail réalisé**

- Analyse du processus: la mise en expédition n’est possible que pour un colis au statut "CRÉER".
- Étude de la documentation pour comprendre l’API à implémenter (`/{parcel-id}/ship`).
- Apprentissage de l’outil Bruno pour le test des APIs.
- Développement de la fonctionnalité:
  - Création d’un contrôleur pour l’API d’expédition.
  - Développement du service métier associé.
  - Rédaction de tests unitaires avec Mockito pour garantir la fiabilité du code.
  - Rédaction et exécution de tests d’acceptance sur Azure pour valider l’intégration.


### Compétences mobilisées

- Analyse fonctionnelle et technique
- Modélisation de base de données
- Développement Java (API, services, tests, Spring Framework)
- Utilisation d’outils de test et d’intégration continue
- Travail en équipe et adaptation à un environnement existant


### Difficultés rencontrées

- Complexité liée au nombre d’outils et de logiciels à maîtriser
- Difficulté à comprendre et tester le code legacy
- Analyse approfondie du code existant nécessaire pour éviter les régressions lors de l’intégration des nouvelles fonctionnalités

---

## Période : 5 - 9 mai

### Tâche: Fiabilisation des informations douanières

**Contexte**

- Problème critique sur les factures à destination de la Réunion
- Anomalie: génération de colis fantômes due à la différence entre le magasin de commande et le lieu de livraison

**Réalisation**

- Analyse approfondie des flux de données entre LOGUP et la Comptabilité
- Identification et correction du bug dans la génération du fichier `.3`
- Mise en place d'une solution
- Tests approfondis en environnement de développement

### Tâche: Suivi des colis en magasin

**Contexte**

- Besoin d'améliorer la traçabilité des colis lors de leur réception en magasin

**Réalisation**

- Développement d'une API REST avec Spring Boot
- Implémentation des règles métier pour la mise à jour des statuts
- Gestion robuste des cas d'erreur
- Mise en place de tests unitaires et d'intégration


### Compétences mobilisées

- Développement: Java, Spring Boot
- Base de données: SQL, analyse de données
- Tests: JUnit, Mockito
- Outils: Debug avancé, Mercurial, Azure DevOps


### Vie en entreprise

- Sprint Review: participation active aux discussions
- Formation sur l'architecture logicielle: meilleure compréhension de l'écosystème technique


### Difficultés surmontées

- Adaptation aux spécificités du code legacy
- Maîtrise des outils de debug complexes
- Compréhension des interactions entre les différents modules (Logistique, Web Commandes etc)

---


## Période : 12 - 16 mai

### Tâche 1: Gestion des fichiers STACI avec encodage spécifique

**Contexte**

- Le traitement intègre des fichiers STACI via la tâche `RECEPTION`.
- Ces fichiers sont fournis au format ISO_8859_1, alors que le code actuel attend à un encodage UTF-8.
- Cela provoque des erreurs lors de l'intégration des fichiers.

**Travail réalisé**

- Analyse du problème
- Lecture des logs d'erreur pour identifier la source du problème.
- Vérification de l'encodage des fichiers STACI
- Mise à jour de les méthodes pour lire les fichiers en utilisant l'encodage.
- Utilisation de `BufferedReader` avec un `InputStreamReader` configuré.
- Création de fichiers de test avec différents encodages (UTF-8 et ISO_8859_1) pour valider la robustesse de la solution.
- Utilisation de Mockito pour simuler les appels et vérifier le bon fonctionnement de la méthode.
- Exécution des tests en environnement de développement pour s'assurer que les fichiers STACI sont correctement intégrés sans erreur.


### Tâche 2: Ajout d’un transporteur à une expédition de colis

**Contexte**

- Les utilisateurs souhaitent pouvoir renseigner un transporteur pour une expédition de colis.
- Cette fonctionnalité doit permettre:
  - Au dépôt: de renseigner l'ID et le nom du transporteur (issu du référentiel).
  - En magasin: de renseigner uniquement le nom du transporteur, avec une option pour l'ID si le transporteur est référencé.
- La mise à jour est limitée aux colis ayant le statut "CRÉER".

**Travail réalisé**

- Conception de l'API
- Développement d'une API REST permettant de mettre à jour les informations du transporteur.
- Implémentation des règles métier pour vérifier que le colis est au statut "CRÉER" avant toute modification.
- Gestion des cas d'usage:
- Validation pour le dêpot.
- Validation pour magasin.
- Création de tests pour valider les différents cas d'usage.
- Simulation des appels API avec Mockito pour vérifier les réponses et les erreurs.
- Intégration de l'API dans l'environnement de développement et exécution des tests pour garantir son bon fonctionnement.


### Tâche 3: Gestion des erreurs pour poids/volume manquant

**Contexte**

- Lorsqu’un poids ou un volume est manquant lors de l’emballage, une erreur "null" s’affiche dans la popup, rendant le problème difficile à diagnostiquer.
- L’objectif est d’afficher un message explicite: "Poids/volume non renseigné pour cet article".

**Travail réalisé**

- Analyse des logs*:
- Étude de la stack trace pour identifier l’origine du exception.
- Localisation du problème dans la méthode et la classe.
- Modification du code
- Ajout de vérifications pour détecter les valeurs nulles avant leur conversion en entier.
- Implémentation d’un message d’erreur explicite dans la méthode concernée.
- Création de scénarios de test pour simuler des cas où le poids ou le volume est manquant.
- Validation que le message d’erreur s’affiche correctement dans la popup.
- Exécution des tests en environnement de développement pour s’assurer que l’erreur est correctement gérée.


### Compétences mobilisées

- Lecture et manipulation de fichiers avec différents encodages, développement d’API REST avec Spring Boot, gestion des exceptions et amélioration des messages d’erreur.
- Création de tests unitaires et d’intégration avec JUnit et Mockito, validation des fonctionnalités en environnement de développement.
- Lecture et interprétation des logs pour identifier les problèmes, conception de solutions robustes adaptées aux besoins métier.


### Difficultés rencontrées

- Adaptation du code pour gérer un encodage différent sans impacter les autres fonctionnalités.
- Gestion des cas d’usage variés pour l’ajout de transporteurs, notamment en magasin où les données sont déclaratives.
- Identification précise des causes des erreurs "null" dans un code existant complexe.

---

## Période : 19 - 23 mai

### Tâche: Reroutage de colis Web en magasin

**Contexte**

En tant qu'agent dépôt au service d'expédition, il est nécessaire de permettre le reroutage d'un colis magasin contenant des commandes web vers le même magasin. Actuellement, le reroutage est bloqué si le colis contient des commandes web, même lorsque le magasin d'arrivée est identique au magasin de départ. Cette limitation empêche la régénération de l'étiquette colis, ce qui est une exigence opérationnelle.

---

**Travail réalisé**

- Analyse de code:
  - Cette méthode gère la logique de reroutage des colis, y compris la création de nouvelles expéditions et la suppression des anciennes.
  - La méthode utilise des processus standards pour la création d'expéditions et la gestion des tâches associées.
- Identification de problème (Le reroutage est bloqué pour les colis contenant des commandes web, même lorsque le magasin d'arrivée est identique au magasin de départ)

- Ajout d'une condition pour autoriser le reroutage si le magasin d'arrivée est identique au magasin de départ, même si le colis contient des commandes web.
- Si le magasin d'arrivée est différent, le reroutage reste bloqué pour les colis contenant des commandes web.
- Ajout d'une étape pour relancer l'impression de l'étiquette colis après la création de la nouvelle expédition.
- Si l'impression automatique n'est pas possible, une indication est ajoutée dans la popup de confirmation pour demander à l'utilisateur de réimprimer l'étiquette manuellement.
- Réorganisation des conditions pour clarifier la logique de reroutage.
- Extraction de certaines parties du code en méthodes privées pour réduire la complexité de `saveFctRoutage`.
- Utilisation de la méthode  pour vérifier la présence de commandes web dans le colis.
- Appel des processus standards pour la suppression des anciennes expéditions et la création des nouvelles.
- Le reroutage est désormais possible pour les colis contenant des commandes web si le magasin d'arrivée est identique au magasin de départ.
- Une nouvelle étiquette colis est générée automatiquement après le reroutage. Si ce n'est pas possible, une notification est affichée pour demander la réimpression manuelle.
- Le code est plus lisible et maintenable grâce au refactoring effectué.

### Tâche: Tracer les erreurs des appels API parcels

## Contexte

L'objectif est de tracer les erreurs des appels API `parcels` afin de pouvoir retrouver les erreurs associées aux contenus des requêtes d'origine. Les données à tracer incluent :

- **Contenu de l'appel (JSON)** : le corps de la requête initiale.
- **Erreur (JSON)** : les détails de l'exception levée.
- **Code d'erreur** : le statut HTTP associé à l'erreur.
- **Date/heure** : le moment où l'erreur s'est produite.

---

## Travail réalisé

### Mise en place d'un filtre pour capturer le contenu des requêtes

- Un filtre personnalisé `RequestCachingFilter` a été implémenté pour envelopper les requêtes avec un `ContentCachingRequestWrapper`. Cela permet de conserver le contenu des requêtes pour les lire ultérieurement, même après leur traitement.
- Ce filtre a été enregistré dans la configuration via la classe `FilterConfig`.

### Gestion des erreurs dans un ErrorHandler global

- Une classe `ErrorHandler` a été créée pour centraliser la gestion des exceptions.
- Lorsqu'une erreur survient :
  - Le contenu de la requête est extrait à l'aide du `ContentCachingRequestWrapper` et désérialisé en objet JSON.
  - Les détails de l'erreur, le code d'erreur et la date/heure sont encapsulés dans un objet `ApiErrorLog`.

### Problème de sérialisation de `LocalDateTime`

- Lors de la sérialisation de l'objet `ApiErrorLog`, un problème est survenu avec le champ `timestamp` de type `LocalDateTime`. Par défaut, Jackson ne sait pas comment sérialiser ce type.
- Solution : Utilisation de la méthode `findAndRegisterModules()` de l'`ObjectMapper` pour enregistrer automatiquement les modules nécessaires, comme le module Java Time.

### Structure des données tracées

- L'objet `ApiErrorLog` a été conçu pour contenir les données à tracer.
- Une méthode `getFormattedTimestamp()` a été ajoutée pour formater la date/heure en chaîne lisible.
- Le contenu de la requête est stocké sous forme d'objet JSON pour éviter les problèmes de double sérialisation.

---

## Cas en succès et en erreur

- **Cas en succès** : Lorsqu'une erreur API est levée, les données (contenu de la requête, erreur, code d'erreur, date/heure) sont correctement tracées et visualisables dans l'outil de journalisation.
- **Cas en erreur** : Si le `ContentCachingRequestWrapper` n'est pas utilisé ou si le filtre n'est pas correctement configuré, le contenu de la requête ne peut pas être lu, ce qui empêche de tracer les erreurs correctement.

---

## Résolution des problèmes

- **Problème de sérialisation de `LocalDateTime`** : Résolu en enregistrant les modules nécessaires avec Jackson.
- **Problème de lecture du contenu de la requête** : Résolu en enveloppant les requêtes avec `ContentCachingRequestWrapper` via le filtre `RequestCachingFilter`.
- **Problème de double sérialisation** : Résolu en stockant le contenu de la requête sous forme d'objet JSON au lieu de chaîne.
