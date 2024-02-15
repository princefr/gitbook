# Diviser pour régner

L'algorithme de diviser pour régner consiste à diviser le problème original en n sous-problèmes de taille plus petite et structurellement similaires au problème original. Ensuite, on résout récursivement ces sous-problèmes, puis on combine leurs résultats pour obtenir la solution du problème original.

Diviser pour régner par rapport à la récursivité : Diviser pour régner est une approche de résolution de problèmes, tandis que la récursivité est une technique de programmation ; Diviser pour régner est généralement mieux mise en œuvre avec la récursivité. Chaque niveau de récursion implique généralement trois opérations : diviser, résoudre et combiner.

Conditions d'application :

- Le problème original et les sous-problèmes partagent un modèle similaire.
- Le problème original et les sous-problèmes peuvent être résolus indépendamment, sans corrélation.
- Il existe une condition d'arrêt pour la division.
- La complexité de la fusion ne doit pas être trop élevée.

[Map-Reduce](../distributed-system/computing.md#mr) utilise également le concept de diviser pour régner.

## Exemple de problème

- [LeetCode 50 : Calcul de la puissance de x à la puissance n.](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/divide/LT50.java)