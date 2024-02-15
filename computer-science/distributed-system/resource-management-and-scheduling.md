# Gestion des Ressources

## Structure Centralisée

Un ou plusieurs serveurs forment un groupe de serveurs centralisés, où toutes les opérations du système sont d'abord traitées par le serveur central. Les différents nœuds de service sont connectés au groupe de serveurs centralisés et rapportent leurs informations au serveur central.

Avantages : Déploiement simple.

![](../../.gitbook/assets/image%20%28274%29.png)

### Borg

![](../../.gitbook/assets/image%20%28273%29.png)

### Kubernetes

![](../../.gitbook/assets/image%20%28275%29.png)

### Mesos

![](../../.gitbook/assets/image%20%28272%29.png)

## Structure Décentralisée

La structure centralisée présente des problèmes de goulot d'étranglement de performances et de pannes ponctuelles. Les structures décentralisées peuvent être mises à l'échelle de manière très importante.

Exemples avec Akka, Redis et Cassandra.

![](../../.gitbook/assets/image%20%28281%29.png)