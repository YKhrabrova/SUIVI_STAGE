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