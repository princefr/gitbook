# Cache

## Scénarios d'utilisation

### Haute performance

Si une requête de base de données (requête complexe, requête sur plusieurs tables ou bases de données) prend beaucoup de temps, et que les données ne changent pas souvent dans un court laps de temps, il est possible de mettre en cache les résultats de la requête.

Étant donné que le cache est en mémoire, les performances sont nettement meilleures que celles de la base de données.

### Haute concurrence

## Inconvénients

### Incohérence entre le cache et la base de données

Modèle Cache Aside :

* Lecture : d'abord dans le cache, puis dans la base de données.
* Mise à jour : d'abord **supprimer** le cache, puis mettre à jour la base de données. La raison de la suppression plutôt que de la mise à jour du cache est que le résultat est souvent le fruit de requêtes ou de calculs complexes, ce qui rend la mise à jour coûteuse. C'est en réalité une approche paresseuse (lazy).

#### Scénario un

**Situation** : modification de la base de données d'abord, puis suppression du cache. Si la suppression du cache échoue, la base de données contiendra de nouvelles données tandis que le cache contiendra les anciennes données, entraînant une incohérence des données.

**Solution** : supprimer d'abord le cache, puis modifier la base de données.

#### Scénario deux

**Situation** : suppression du cache d'abord, puis tentative de modification de la base de données. À ce stade, la modification de la base de données n'est pas terminée. Une nouvelle requête arrive, recherche le cache, trouve qu'il est vide, accède à la base de données et récupère les anciennes données avant modification, qu'elle met dans le cache. Pendant ce temps, la modification de la base de données est finalisée.

Solution :

### Avalanche de cache

Si le cache tombe en panne, toutes les requêtes sont instantanément redirigées vers la base de données, entraînant un crash de la base de données.

Solutions :

* Assurer une haute disponibilité du cache.
* Cache interne au système, par exemple Ehcache.
* Limiter et dégrader le trafic au niveau de la couche de base de données. Par exemple, Hystrix.
* Persistance du cache pour une récupération rapide.

### Perforation du cache

En raison de requêtes incorrectes (comme des attaques de pirates informatiques), toutes les requêtes ne peuvent pas accéder au cache, entraînant un accès direct à la base de données. Par exemple, une requête avec un ID de -1 alors que la base de données contient des IDs de 1 à 100.

Il est possible d'écrire une valeur nulle dans le cache pour les données non trouvées dans la base de données.

### Concurrence dans le cache

**Situation** : plusieurs services mettent à jour simultanément une clé dans le cache, sans respecter l'ordre prévu, ce qui entraîne finalement des données de cache incorrectes.

**Solution** : Verrouillage distribué + horodatage.

![](../../.gitbook/assets/01redis-bing-fa-jing-zheng-wen-ti-yi-ji-jie-jue-fang-an.png)