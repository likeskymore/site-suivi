---
title: Suivi du projet
---

<style>
    @media screen and (min-width: 76em) {
        .md-sidebar--primary {
            display: none !important;
        }
    }
</style>

# Suivi de projet

## Semaine 1 — (1-6 Septembre)

### Objectifs de la période
- Prendre connaissance de l’architecture existante de PromptStudio.
- Comprendre les composants hérités de ChainForge.
- Identifier les points d’intégration pour les évaluateurs et les processeurs.

### Travail réalisé

!!! abstract "Avancement"
    - [x] Exploration du code source et des implémentations existantes.
    - [x] Familiarisation avec les flux d’exécution de PromptStudio.
    - [x] Repérage des fonctions de conversion et d’exécution à réutiliser.

### Décisions et ajustements

!!! info "Décisions"
    - Conserver les mécanismes existants de ChainForge lorsque ceux-ci répondent déjà aux besoins du projet.
    - Intégrer progressivement les nouveaux évaluateurs et processeurs dans l’architecture actuelle.

### Difficultés rencontrées

!!! warning "Difficultés"
    - La prise en main a nécessité l’analyse de plusieurs couches du projet avant de pouvoir isoler les points d’intégration.

## Semaine 2 — (7-13 Septembre)

### Objectifs de la période
- Intégrer les évaluateurs Simple et Python.
- Adapter la conversion `jsonToYml` aux nouveaux cas.
- Relier les évaluateurs au moteur d’exécution.

### Travail réalisé

!!! abstract "Avancement"
    - [x] Adaptation de `jsonToYml` pour gérer les nouveaux types d’évaluateurs.
    - [x] Implémentation de `execute_python` avec exécution par sous-processus et enveloppe JSON.
    - [x] Génération du code JavaScript de l’évaluateur Simple avec `createJSEvalCodeFor`.
    - [x] Réutilisation de `execute_javascript` pour exécuter l’évaluateur Simple.
    - [x] Intégration des deux évaluateurs dans `worker.ts`.

### Décisions et ajustements

!!! info "Décisions"
    - Réutiliser les fonctions d’exécution existantes afin de limiter la duplication de code.
    - Conserver une conversion commune des configurations avant leur exécution.

### Difficultés rencontrées

!!! warning "Difficultés"
    - Les nouveaux cas d’évaluateurs exigeaient d’étendre `jsonToYml` sans modifier le comportement des types déjà supportés.

## Semaine 3 — (14-20 Septembre)

### Objectifs de la période
- Ajouter les processeurs ChainForge nécessaires à PromptStudio.
- Adapter le schéma de données aux nouveaux types de processeurs.
- Vérifier le comportement de l’application sur le cluster.

### Travail réalisé

!!! abstract "Avancement"
    - [x] Adaptation de `jsonToYml` pour les nouveaux types de processeurs.
    - [x] Ajout de la colonne optionnelle `format` pour les processeurs Split.
    - [x] Ajout de la colonne optionnelle `code` pour les processeurs contenant du code.
    - [x] Création d’un énuméré dédié aux types de processeurs.
    - [x] Implémentation de `execute_split` avec la fonction `splitText` de ChainForge.
    - [x] Réutilisation de `execute_python` pour le processeur Python.
    - [x] Intégration des processeurs dans `worker.ts`.
    - [x] Validation de l’application sur le serveur madagh du DIRO.

### Décisions et ajustements

!!! info "Décisions"
    - Séparer les types de processeurs des types d’évaluateurs dans le modèle de données.
    - Utiliser un tunnel pour rendre MySQL accessible au cluster pendant les tests.

### Difficultés rencontrées

!!! warning "Difficultés"
    - Le déploiement sur le cluster nécessitait d’établir un tunnel entre le poste local et le serveur madagh.
    - Le schéma devait rester compatible avec les processeurs qui ne possèdent ni format ni code.

## Semaine 4 — (21-27 Septembre)

### Objectifs de la période
- Corriger le calcul des jetons d’entrée d’une expérience.
- Vérifier le comportement du calcul pour les nouvelles expériences.

### Travail réalisé

!!! abstract "Avancement"
    - [x] Analyse du cas où le nombre de jetons d’entrée était toujours égal à zéro.
    - [x] Identification de la condition liée à `final_dataset_id`.
    - [x] Correction du calcul afin qu’il soit effectué même lorsque `final_dataset_id` est nul.

### Décisions et ajustements

!!! info "Décisions"
    - Le calcul doit dépendre des données d’entrée disponibles plutôt que de l’existence préalable d’un jeu de données final.

### Difficultés rencontrées

!!! warning "Difficultés"
    - Les nouvelles expériences initialisaient `final_dataset_id` à `null`, ce qui empêchait prématurément le calcul.

## Semaine 5 — (28 Septembre-4 Octobre)

### Objectifs de la période
- Ajouter les évaluateurs LLM et Multi-Evaluator.
- Étendre le pipeline de conversion et le moteur d’exécution.

### Travail réalisé

!!! abstract "Avancement"
    - [x] Ajout d’un bloc de configuration pour les évaluateurs de type `llm` dans `jsonToYml`.
    - [x] Implémentation de l’exécution LLM avec remplacement de `{response}` et appel à `queryLLM`.
    - [x] Ajout d’un bloc de configuration pour les évaluateurs de type `multieval`.
    - [x] Première implémentation de l’exécution des évaluateurs enfants.
    - [x] Intégration de la logique correspondante dans `worker.ts`.

### Décisions et ajustements

!!! info "Décisions"
    - La première version du Multi-Evaluator exécutait directement les évaluateurs enfants au lieu de traiter le groupe comme une structure récursive.
    - Les évaluateurs enfants étaient temporairement repérés par une convention de nommage de la forme `{nom}-{compteur}`.

### Difficultés rencontrées

!!! warning "Difficultés"
    - L’approche initiale pour le Multi-Evaluator contournait sa structure réelle.
    - Après discussion avec le professeur Syriani, cette solution a été jugée inefficace et a dû être repensée.

## Semaine 6 — (5-11 Octobre)

### Objectifs de la période
- Refactoriser la conversion `jsonToYml`.
- Persister séparément les configurations LLM et Multi-Evaluator.
- Permettre l’exécution récursive des évaluateurs.

### Travail réalisé

!!! abstract "Avancement"
    - [x] Extraction de `buildYmlNode` pour séparer la construction YAML et l’analyse JSON.
    - [x] Ajout du traitement récursif des nœuds Multi-Evaluator.
    - [x] Introduction de `multievalChildToNodeFormat` pour normaliser les évaluateurs enfants.
    - [x] Création des tables `Llm_evaluator`, `Multi_evaluator` et `Multi_evaluator_mapping`.
    - [x] Implémentation de `handle_save_llm_evaluator` et `save_evaluator`.
    - [x] Mise à jour de `save_config` et de `handle_save_evaluator`.
    - [x] Adaptation de `worker.ts` pour résoudre et exécuter récursivement les évaluateurs.

### Décisions et ajustements

!!! info "Décisions"
    - Les Multi-Evaluators sont représentés à la fois comme évaluateurs génériques et comme structures spécialisées.
    - Les relations entre un Multi-Evaluator et ses enfants sont persistées dans une table de correspondance dédiée.
    - Les configurations LLM sont séparées des autres évaluateurs afin de conserver un modèle de données clair.

### Difficultés rencontrées

!!! warning "Difficultés"
    - Les configurations d’évaluateurs enfants ne possédaient pas toujours la même structure que les évaluateurs autonomes.
    - La persistance devait retourner l’identifiant du nœud parent pour construire correctement les correspondances.

## Semaine 7 — (12-18 Octobre)

### Objectifs de la période
- Résoudre les entrées avant leur envoi aux processeurs Split et Join.
- Implémenter le processeur Join.
- Conserver la traçabilité des entrées résolues et des résultats.

### Travail réalisé

!!! abstract "Avancement"
    - [x] Mise à jour de `EvaluatorRunner.ts` pour transmettre les entrées résolues.
    - [x] Création de la table `resolved_input`.
    - [x] Ajout de la clé étrangère `resolved_input_id` dans `processor_result`.
    - [x] Réutilisation du pipeline d’entrées résolues pour le processeur Join.
    - [x] Regroupement des entrées selon `selected_group_vars`.
    - [x] Association des résultats Join à une entrée contributrice.

### Décisions et ajustements

!!! info "Décisions"
    - Une valeur comme `this is a {text}` doit être résolue avant d’être envoyée au processeur.
    - La première version du processeur Join se limite au flux `Dataset → Join Processor`.

### Difficultés rencontrées

!!! warning "Difficultés"
    - Les résultats Join sont encore associés à l’identifiant d’une seule entrée contributrice.
    - Une structure dédiée pour mapper toutes les entrées, leurs marqueurs et les résultats a été envisagée, mais reportée en raison de sa complexité.
    - Les autres types de nœuds en amont d’un Join ne sont pas encore pris en charge.

<!-- ## Semaine 8 — (19-25 Octobre)

### Objectifs de la période
- Tester les fonctionnalités issues de ChainForge.
- Comparer les résultats de PromptStudio avec les flux d’exemple.
- Repérer les comportements non supportés.

### Travail réalisé

!!! abstract "Avancement"
    - [x] Utilisation des flux d’exemple de ChainForge comme jeux de référence.
    - [x] Vérification de plusieurs structures d’exécution et cas d’utilisation.
    - [x] Identification d’un problème avec les valeurs de comparaison variables ou metavars du Simple Evaluator.

### Décisions et ajustements

!!! info "Décisions"
    - Les flux d’exemple de ChainForge servent de référence pour valider progressivement les fonctionnalités importées.
    - Le support des variables et metavars doit être traité dans la conversion et l’exécution, plutôt que dans le seul formulaire de configuration.

### Difficultés rencontrées

!!! warning "Difficultés"
    - Le Simple Evaluator supposait que la valeur de comparaison était toujours fournie directement par l’utilisateur.
    - Les metavars étaient présentes dans le pipeline, mais ne permettaient pas encore de distinguer correctement les valeurs à utiliser. -->

<!-- ## Semaine 9 — (26 Octobre-1 Novembre)

### Objectifs de la période
- Corriger la gestion des variables et metavars du Simple Evaluator.
- Initialiser le frontend du tableau de bord de monitoring.

### Travail réalisé

!!! abstract "Avancement"
    - [x] Adaptation de `jsonToYml` pour exporter les métadonnées des variables et metavars.
    - [x] Construction des dictionnaires `vars` et `meta` dans `worker.ts`.
    - [x] Initialisation du répertoire `frontend`.
    - [x] Mise en place de Next.js, TypeScript et Tailwind CSS.

### Décisions et ajustements

!!! info "Décisions"
    - Les variables et metavars sont distinguées à partir des métadonnées déjà présentes dans `markerDict`.
    - Le frontend est développé comme un tableau de bord dédié au suivi des expériences.

### Difficultés rencontrées

!!! warning "Difficultés"
    - Les données nécessaires étaient déjà transmises dans `markerDict`, mais leur distinction n’était pas conservée lors de la conversion.
    - La formulation du code du Simple Evaluator restait incorrecte dans certains cas et devait être reprise la semaine suivante. -->

<!-- ## Semaine 10 — (2-8 Novembre)

### Objectifs de la période
- Finaliser la formulation du code du Simple Evaluator.
- Avancer le frontend du tableau de bord.
- Clarifier les responsabilités entre le frontend et le backend.

### Travail réalisé

!!! abstract "Avancement"
    - [x] Ajout d’une vérification du type de valeur avant la génération du code JavaScript.
    - [x] Correction des références aux variables et metavars dans le code généré.
    - [x] Création de la page d’accueil du tableau de bord.
    - [x] Ajout des points d’entrée vers les pages de configuration d’expérience et d’analyse.
    - [x] Connexion du backend Express au frontend Next.js pour la lecture des données.

### Décisions et ajustements

!!! info "Décisions"
    - Le frontend doit être en lecture seule et ne doit pas écrire directement dans la base de données.
    - La page Prompt Designer a été considérée comme redondante et sera retirée.

### Difficultés rencontrées

!!! warning "Difficultés"
    - La première formulation du code ne vérifiait pas si la valeur sélectionnée était une variable ou une metavariable avant de générer le JavaScript.
    - L’architecture initiale du frontend permettait trop directement les opérations d’écriture et devait être réorientée. -->

<!-- ## Semaine 11 — (9-15 Novembre)

### Objectifs de la période
- Mettre en place le suivi en temps réel des expériences.
- Choisir un mécanisme de diffusion adapté au besoin du tableau de bord.
- Centraliser l’état d’exécution d’une expérience.

### Travail réalisé

!!! abstract "Avancement"
    - [x] Comparaison des approches WebSocket et Server-Sent Events.
    - [x] Choix de SSE pour la communication unidirectionnelle du backend vers le frontend.
    - [x] Définition des endpoints SSE.
    - [x] Implémentation de `ExperimentRunState` avec les métriques d’exécution principales.
    - [x] Création de `runState.ts` pour créer, mettre à jour, persister et observer les états.
    - [x] Mise à jour de `experimentRunner.ts` pour publier la progression.
    - [x] Implémentation de la réception des événements SSE dans les services frontend.

### Décisions et ajustements

!!! info "Décisions"
    - SSE a été retenu parce que seul le backend doit pousser les mises à jour vers le frontend.
    - Un mécanisme d’observation relie les changements d’état aux endpoints SSE.

### Difficultés rencontrées

!!! warning "Difficultés"
    - L’état devait être mis à jour tout au long de l’exécution sans perdre les informations sur les requêtes réussies, échouées ou réessayées. -->

<!-- ## Semaine 12 — (16-22 Novembre)

### Objectifs de la période
- Construire le contenu principal de la page de monitoring.
- Visualiser la progression et les performances d’une expérience.
- Limiter la fréquence des mises à jour SSE.

### Travail réalisé

!!! abstract "Avancement"
    - [x] Création du composant réutilisable `MetricCard`.
    - [x] Ajout de graphiques avec ECharts.
    - [x] Récupération et surveillance des mises à jour de l’expérience sélectionnée.
    - [x] Définition des couleurs pour les états `completed`, `running`, `queued`, `failed` et `paused`.
    - [x] Ajout d’un mécanisme de throttling pour les snapshots SSE.
    - [x] Conservation du dernier état lorsque l’intervalle de diffusion n’est pas écoulé.

### Décisions et ajustements

!!! info "Décisions"
    - Le frontend reçoit le dernier état connu à intervalles contrôlés, plutôt que chaque changement individuel.
    - Les mêmes statuts sont utilisés de manière cohérente dans les cartes et les graphiques.

### Difficultés rencontrées

!!! warning "Difficultés"
    - Les mises à jour fréquentes de l’état pouvaient produire un trafic réseau et un traitement excessifs côté frontend. -->

<!-- ## Semaine 13 — (23-29 Novembre)

### Objectifs de la période
- Compléter les indicateurs de performance du monitoring.
- Calculer les métriques à partir de l’état courant de l’expérience.

### Travail réalisé

!!! abstract "Avancement"
    - [x] Ajout des métriques de nombre total de requêtes et de jetons utilisés.
    - [x] Ajout des métriques de jetons par requête et de requêtes par minute.
    - [x] Ajout de la métrique de jetons par minute.
    - [x] Mise à jour des cartes pendant l’exécution de l’expérience.

### Décisions et ajustements

!!! info "Décisions"
    - Les métriques sont calculées à partir de la progression et du temps d’exécution du run sélectionné.
    - Les cartes de métriques complètent les graphiques sans remplacer la visualisation de la progression.

### Difficultés rencontrées

!!! warning "Difficultés"
    - Les indicateurs doivent rester cohérents pendant qu’une expérience est encore en cours et que ses valeurs évoluent. -->

<!-- ## Semaine 14 — (30 Novembre-6 Décembre)

### Objectifs de la période
- Enrichir le monitoring avec les modèles utilisés.
- Ajouter des indicateurs de latence pour les requêtes LLM.

### Travail réalisé

!!! abstract "Avancement"
    - [x] Ajout de la liste des modèles LLM utilisés par l’expérience sélectionnée.
    - [x] Connexion à l’API backend pour récupérer les modèles associés à l’expérience.
    - [x] Ajout du suivi de la latence des requêtes LLM.
    - [x] Ajout des statistiques de latence moyenne, P50, P95 et P99.

### Décisions et ajustements

!!! info "Décisions"
    - Les latences sont présentées avec plusieurs percentiles afin de distinguer le comportement moyen des cas plus lents.
    - Les informations sur les modèles sont affichées dans la même page que les métriques d’exécution.

### Difficultés rencontrées

!!! warning "Difficultés"
    - Les données de latence doivent être collectées et agrégées sans perturber le suivi de progression en temps réel. -->

<!-- ## Semaine 15 — (7-13 Décembre)

### Objectifs de la période
- Poursuivre la validation du tableau de bord.
- Documenter les prochaines améliorations à partir des résultats de tests.

### Travail réalisé

!!! abstract "Avancement"
    - [ ] Aucun travail documenté dans le rapport de référence pour cette période.

### Décisions et ajustements

!!! info "Décisions"
    - Les prochaines tâches seront précisées après la poursuite des tests du monitoring.

### Difficultés rencontrées

!!! warning "Difficultés"
    - Aucun obstacle supplémentaire n’est documenté pour cette période. -->

<!-- ## Semaine 16 — (14-20 Décembre)

### Objectifs de la période
- Finaliser le suivi de la session et préparer le bilan du projet.

### Travail réalisé

!!! abstract "Avancement"
    - [ ] Aucun travail documenté dans le rapport de référence pour cette période.

### Décisions et ajustements

!!! info "Décisions"
    - La finalisation sera complétée lorsque les derniers résultats et validations seront disponibles.

### Difficultés rencontrées

!!! warning "Difficultés"
    - Aucun obstacle supplémentaire n’est documenté pour cette période. -->
