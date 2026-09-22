---
title: Vue d'ensemble du projet
---

<style>
    @media screen and (min-width: 76em) {
        .md-sidebar--primary {
            display: none !important;
        }
    }
</style>

# Vue d'ensemble du projet

!!! info "Informations générales"
    **Session**: Automne 2026  
    **Auteur(s)**: Trung Nguyen (20238006)  
    **Thème(s)**: Exécution et suivi d'expériences dans PromptStudio  
    **Superviseur(s)**: Prof. Eugene Syriani  
    **Collaborateur(s):** N/A  

## Description du projet

PromptStudio est un outil conçu pour assister les ingénieurs en prompts dans la conception, l'exécution et l'évaluation d'expériences sur des grands modèles de langage. Son objectif principal est d'unifier le cycle de développement des prompts avec les tâches d'exécution, de mesure et de diagnostic des résultats, afin de rendre ce processus plus structuré, reproductible et exploitable.

### Contexte

ChainForge est une plateforme open source dédiée à l'expérimentation avec les modèles de langage. Elle permet de construire des workflows à partir de prompts, d'exécuter ces workflows avec différents modèles et configurations, puis de comparer les résultats obtenus. Elle offre ainsi un environnement pour concevoir, tester et analyser des expériences impliquant plusieurs requêtes et modèles de langage.

PromptStudio vise à mettre à l'échelle et à automatiser ce type d'expérimentations. Le projet consiste notamment à intégrer certaines fonctionnalités de ChainForge dans PromptStudio afin de permettre l'exécution automatisée d'expériences comportant un grand nombre de tâches, de configurations et de conditions de test. Cette intégration cherche à préserver les capacités fondamentales d'expérimentation de ChainForge tout en les adaptant à une architecture conçue pour des exécutions plus automatisées, extensibles et observables.

Dans ce contexte, un tableau de bord de suivi des expériences est également développé pour rendre leur exécution plus transparente et plus facile à surveiller. Il permet de suivre en temps réel la progression des expériences, l'état des tâches, les requêtes exécutées, les erreurs et plusieurs métriques d'exécution. L'objectif est de fournir une interface permettant aux utilisateurs de lancer et de surveiller des workflows d'expérimentation à grande échelle directement depuis PromptStudio.

### Problématique

Les outils de conception et d'exécution d'expériences LLM, comme ChainForge, offrent déjà des mécanismes utiles pour définir des workflows, exécuter des prompts sur plusieurs modèles et comparer des résultats. Toutefois, ces capacités sont souvent limitées lorsqu'il s'agit de déployer ces workflows dans un environnement plus évolutif, plus automatisé et mieux intégré à l'écosystème de PromptStudio. Le principal enjeu réside donc moins dans la simple exécution des prompts que dans l'intégration fonctionnelle des principes de ChainForge dans une plateforme plus générale dédiée à l'orchestration et au suivi des expériences.

À ce niveau, les limitations ne se situent pas uniquement dans le suivi des résultats, mais dans le fait que les workflows d'expérimentation ne sont pas suffisamment intégrés à une vue globale de progression, de diagnostic et de contrôle. Les expérimentateurs doivent souvent jongler entre plusieurs outils ou entre des vues trop générales, ce qui complique la compréhension de l'état réel d'une exécution. Cette difficulté est particulièrement marquée lorsque les expériences s'étendent sur plusieurs heures ou jours, avec de nombreuses variantes, modèles et fournisseurs, et lorsque les erreurs ou les ralentissements ne sont pas immédiatement visibles.

Le problème central est donc la difficulté d'intégrer des fonctionnalités de ChainForge dans PromptStudio de manière cohérente, tout en conservant la capacité de suivre finement les unités logiques d'une expérience, les appels API réels, la latence, l'état d'exécution et les erreurs. Sans cette intégration, l'exécution des expériences reste fragmentée, difficile à superviser et peu adaptée à des workflows de recherche ou de validation à grande échelle.

### Proposition et objectifs

Ce projet propose de transférer et d'intégrer les fonctionnalités clés de ChainForge dans PromptStudio, puis d'ajouter un tableau de bord de suivi comme couche complémentaire de supervision et d'observabilité.

Les objectifs principaux du projet sont les suivants :

- intégrer les fonctionnalités essentielles de ChainForge dans PromptStudio pour supporter la définition, l'exécution et la comparaison d'expériences à partir de workflows de prompts ;
- préserver la logique de conception expérimentale de ChainForge tout en l'adaptant à une architecture plus automatisée et extensible ;
- mettre en place un tableau de bord qui affiche rapidement l'état de progression, la latence et l'intégrité de l'exécution des expériences;

### Méthodologie

La démarche de projet s'articule autour d'une analyse préalable des fonctionnalités clés de ChainForge, de leur intégration dans PromptStudio et d'itérations de validation sur des scénarios représentatifs d'expérimentation LLM.

Dans un premier temps, l'étude porte sur les capacités de ChainForge à créer des workflows, exécuter des prompts sur plusieurs modèles et comparer des résultats, ainsi que sur les limites de leur intégration dans une plateforme plus générale comme PromptStudio. Cette étape permet de définir les éléments à transférer, à adapter et à orchestrer au sein de l'architecture du système. Ensuite, le projet propose un tableau de bord permettant de suivre l’exécution des expériences à travers différentes métriques, notamment la progression, la latence et l’utilisation des tokens.

La méthode repose sur des itérations de conception et de validation centrées sur l’intégration des fonctionnalités de ChainForge dans PromptStudio. Les principaux éléments abordés comprennent la modélisation des workflows, la gestion des différentes unités d’exécution, la prise en charge de plusieurs fournisseurs et modèles, ainsi que le suivi des expériences. Les fonctionnalités développées sont ensuite testées à l’aide de scénarios d’utilisation représentatifs d’exécutions d’expériences avec des LLM, afin de vérifier leur bon fonctionnement dans PromptStudio et leur intégration avec le système d’exécution et le tableau de bord de suivi.

### Validation et Évaluation

La validation du projet se fera à travers des scénarios d’utilisation représentant des expériences de taille intermédiaire ou grande, avec plusieurs modèles et configurations. L’évaluation portera sur la capacité de PromptStudio à intégrer et exécuter les workflows de ChainForge, ainsi qu’à fournir une vue claire de leur progression et de leurs performances.

Les principaux critères d’évaluation seront :

- l’intégration des fonctionnalités essentielles de ChainForge dans PromptStudio ;
- la fiabilité de l’exécution des workflows avec plusieurs modèles et configurations ;
- la clarté du suivi de la progression des expériences ;
- la pertinence des métriques de latence et d’utilisation des tokens ;
- la capacité du tableau de bord à fournir une vue d’ensemble de l’exécution des expériences.

La validation vise ainsi à vérifier que les fonctionnalités intégrées dans PromptStudio fonctionnent correctement et que le tableau de bord fournit les informations nécessaires pour suivre efficacement l’exécution des expériences.

## Échéancier

!!! info
    Le suivi complet est disponible dans la page [Suivi de projet](suivi.md).

| Activités | Début | Fin | Livrable | Statut |
| --- | --- | --- | --- | --- |
| Exploration du codebase et intégration initiale des fonctionnalités de ChainForge dans PromptStudio | 01 sept. | 28 sept. | Évaluateurs, processeurs et calcul des tokens intégrés | 🔄 En cours |
<!-- | Architecture des évaluateurs LLM et Multi-Evaluator | 01 oct. | 14 oct. | Architecture récursive et persistance spécialisées | ✅ Terminé |
| Intégration des processeurs et validation backend | 15 oct. | 28 oct. | Flux Dataset/Split/Join et tests ChainForge validés | ✅ Terminé |
| Support des variables et développement initial du frontend | 01 nov. | 14 nov. | Support des metavars et base du tableau de bord | ⏳ À venir |
| Suivi en temps réel et page de monitoring | 15 nov. | 28 nov. | Flux SSE, ExperimentRunState et visualisations | ✅ Terminé |
| Métriques de performance et de latence | 01 déc. | 14 déc. | Modèles utilisés, tokens et latences P50/P95/P99 | ✅ Terminé | -->
