# Hashage Consistant

Si nous avons un total de trois nœuds.

## Algorithme de Hashage Original

Après la perte d'un nœud, le nombre total change et les données invalides dépassent 1/3.

## Algorithme de Hashage Consistant

Après la perte d'un nœud, les données invalides sont égales à 1/3. Toutes les requêtes de données invalides sont redirigées vers un seul nœud.

## Nœuds Virtuels

Après la perte d'un nœud, les données invalides sont égales à 1/3. Toutes les requêtes de données invalides sont réparties entre les différents nœuds restants.

## Fente de Hashage

Utilisé par Redis.