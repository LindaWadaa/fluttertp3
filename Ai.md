# Prompts propres du TP Flutter - Waiting Room

Ce document regroupe les prompts utilisés pour réaliser le TP, réécrits de
manière claire et réutilisable, dans l'ordre des étapes.

## Objectif général

Créer une application Flutter de salle d'attente permettant :

- d'ajouter un client à une file d'attente ;
- d'afficher le nombre de clients présents ;
- de supprimer un client ;
- de tester la logique métier et l'interface avec Flutter Test ;
- d'expérimenter une gestion d'état avec `Provider`, lorsque la dépendance est
  correctement configurée.

---

## Étape 1 - Créer le projet Flutter

### Prompt

> Crée une application Flutter nommée `waiting_room_app`.
> Configure un projet minimal avec une page principale et vérifie que
> l'application démarre correctement. Conserve une structure standard Flutter,
> utilise Material Design et prépare le projet pour des tests unitaires et des
> tests de widgets.

### Vérifications

```bash
flutter pub get
flutter analyze
flutter test
flutter run
```

---

## Étape 2 - Définir le besoin fonctionnel

### Prompt

> Transforme l'application Flutter en application locale de salle d'attente.
> Ajoute un champ de saisie permettant d'entrer le nom d'un client et un bouton
> `Add`. Affiche les clients ajoutés dans une liste ainsi que le texte
> `Clients in Queue: N`. Ignore les noms vides et ajoute un bouton `Remove`
> pour chaque client. Utilise une interface Material simple et accessible.

### Résultat attendu

- Un nom non vide est ajouté une seule fois par clic sur `Add`.
- La liste et le compteur sont mis à jour immédiatement.
- Un client peut être supprimé.
- Une saisie vide ne modifie pas la file.

---

## Étape 3 - Extraire la logique métier

### Prompt

> Sépare la logique de gestion de la file d'attente de l'interface Flutter.
> Crée une classe `WaitingRoomManager` dans
> `lib/waiting_room_manager.dart` avec une liste privée de clients et les
> opérations suivantes : consulter les clients, ajouter un client, supprimer un
> client par son nom et supprimer un client par son index. Protège la méthode
> de suppression par index contre les index invalides. Adapte `main.dart` pour
> utiliser cette classe sans changer le comportement visible.

### Tests à ajouter

> Écris des tests unitaires pour vérifier l'ajout, la suppression par nom, la
> suppression par index et le comportement avec un index invalide. Ajoute aussi
> des tests de widgets pour vérifier l'ajout et la suppression depuis
> l'interface.

---

## Étape 4 - Ajouter le passage au client suivant

### Prompt

> Ajoute à la gestion de la salle d'attente une opération `nextClient`.
> Cette opération doit retirer le premier client de la file lorsqu'elle n'est
> pas vide et ne doit provoquer aucune erreur lorsque la file est vide.
> Ajoute un bouton d'interface permettant d'appeler cette opération et couvre
> les deux cas par des tests unitaires et des tests de widgets.

---

## Étape 5 - Introduire Provider avec TDD

### Prompt

> Remplace la gestion d'état locale de l'écran par un `ChangeNotifier` nommé
> `QueueProvider`, placé dans `lib/queue_provider.dart`. Il doit exposer la
> liste des clients et fournir `addClient`, `removeClientAt` et `nextClient`.
> Chaque modification doit appeler `notifyListeners()`. Utilise
> `ChangeNotifierProvider`, `context.watch` et `context.read` dans
> `main.dart`. Commence par écrire les tests du provider, puis implémente le
> code jusqu'à ce que tous les tests passent. Ne modifie pas le comportement
> fonctionnel de l'application.

### Tests à prévoir

> Ajoute des tests pour l'état initial, l'ajout d'un client, la suppression par
> index, le passage au client suivant, la file vide et la notification des
> listeners. Ajoute ou adapte les tests de widgets pour valider l'intégration
> avec Provider.

---

## Étape 6 - Corriger la configuration des dépendances

### Prompt

> Vérifie `pubspec.yaml` et corrige la configuration de la dépendance
> `provider`. Comme `provider` est importé par le code de production situé dans
> `lib/`, il doit être déclaré sous `dependencies`, et non sous
> `dev_dependencies`. Utilise une contrainte compatible avec le SDK Dart du
> projet, exécute `flutter pub get`, puis vérifie `flutter analyze` et
> `flutter test`.

### Erreur d'adaptation rencontrée au début du TP

Pendant l'adaptation de l'atelier Provider, la dépendance a été placée sous
`dev_dependencies` dans `pubspec.yaml` :

```yaml
dev_dependencies:
  provider: ^6.0.0
```

C'est une mauvaise classification pour une dépendance utilisée par le code de
production. La déclaration correcte est :

```yaml
dependencies:
  provider: ^6.0.0
```

La règle à retenir est la suivante :

- `dependencies` : packages nécessaires à l'application dans `lib/` et lors de
  l'exécution ;
- `dev_dependencies` : packages utilisés uniquement pour les tests, l'analyse,
  le lint ou les outils de développement.

Après chaque modification de `pubspec.yaml`, il faut régénérer la résolution
des packages :

```bash
flutter pub get
```

Puis contrôler la configuration :

```bash
flutter pub deps
flutter pub outdated
flutter analyze
flutter test
```

> Remarque : selon la version de Flutter/Dart installée, une contrainte de SDK
> trop récente peut aussi provoquer un échec de résolution. Dans ce cas,
> adapter `environment.sdk` à la version réellement installée, sans choisir
> une version arbitraire. La version du SDK doit être vérifiée avec
> `flutter --version`.

---

## Étape 7 - Revenir à une implémentation locale si Provider n'est pas requis

### Prompt

> Si l'objectif final du TP ne nécessite pas Provider, simplifie
> l'application en revenant à `WaitingRoomManager` et à un état local dans
> l'écran. Supprime les imports Provider inutilisés, conserve les mêmes
> fonctionnalités et garde les tests de la logique métier et de l'interface.
> Vérifie qu'aucune dépendance inutilisée ne reste dans `pubspec.yaml`.

Cette étape correspond à une adaptation possible : la version finale peut
utiliser `WaitingRoomManager` sans `QueueProvider`, tout en conservant les
tests de `queue_provider_test.dart` uniquement si l'atelier Provider doit être
conservé. Sinon, ce test doit être supprimé ou remplacé par un test de la
gestion locale.

---

## Étape 8 - Validation finale et publication

### Prompt

> Effectue la validation finale du projet Flutter. Lance les tests, l'analyse
> statique et le formatage. Corrige uniquement les erreurs liées à
> l'implémentation du TP. Vérifie que les fichiers générés et les dossiers de
> build ne sont pas ajoutés inutilement au dépôt. Prépare ensuite un commit
> clair et publie la branche `main` vers le dépôt GitHub
> `LindaWadaa/fluttertp3`.

### Commandes

```bash
dart format .
flutter analyze
flutter test
git status
git add .
git commit -m "Complete waiting room Flutter TP"
git push -u origin main
```

### Contrôles avant le push

- Le dépôt distant est bien :
  `https://github.com/LindaWadaa/fluttertp3.git`.
- Les changements locaux sont compris et validés.
- Aucun secret ou fichier sensible n'est versionné.
- Les tests et l'analyse statique passent.
- Le message de commit décrit clairement la modification.

---

## Résumé des commandes essentielles

```bash
flutter pub get
dart format .
flutter analyze
flutter test
git status
git remote -v
git push -u origin main
```

