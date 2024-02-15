# Ordonnancement

Trouver un serveur approprié pour une tâche s'appelle l'ordonnancement.

## Ordonnancement Monolithique

Dans un cluster, un seul nœud exécute le processus d'ordonnancement. Le planificateur gère à la fois les tâches et les ressources, ayant une vue globale des ressources et des tâches.

Les algorithmes d'ordonnancement comprennent généralement deux phases :

* Vérification de la faisabilité, trouver plusieurs machines pouvant exécuter la tâche.
* Évaluation, choisir une machine parmi plusieurs. Il existe deux variantes : le "match le pire" et le "match le meilleur".

![](../../.gitbook/assets/image%20%28282%29.png)

S'il y a plusieurs clusters, le monolithique est implémenté à travers une **fédération de clusters**. La fédération de clusters est une implémentation en couches de l'ordonnancement monolithique.

## Ordonnancement à Deux Niveaux

La première couche de l'ordonnancement à deux niveaux est le planificateur central, responsable uniquement de la gestion et de l'allocation des ressources ; la deuxième couche n'a qu'une vue partielle des ressources et est responsable de l'association des tâches aux ressources.

![](../../.gitbook/assets/image%20%28283%29.png)

## Ordonnancement avec État Partagé

Il existe plusieurs planificateurs partageant l'état du cluster, comprenant l'état des ressources et des tâches. Les conflits sont résolus par un ordonnancement concurrentiel optimiste, tandis que l'ordonnancement à deux niveaux est pessimiste.

![](../../.gitbook/assets/image%20%28279%29.png)

## Comparaison

![](../../.gitbook/assets/image%20%28284%29.png)

![](../../.gitbook/assets/image%20%28278%29.png)