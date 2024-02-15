# Algorithme

**La structure de données fait référence à un ensemble de structures de stockage de données**, telles que des files d'attente, des piles, des tas, **l'algorithme est un ensemble de méthodes pour manipuler les données**, telles que la recherche binaire, la programmation dynamique.

Les structures de données et les algorithmes sont complémentaires, **les structures de données servent les algorithmes, et les algorithmes doivent agir sur des structures de données spécifiques**. Par exemple, les tableaux ont la caractéristique d'un accès aléatoire, la recherche binaire nécessite l'utilisation de tableaux pour stocker les données, si des listes chaînées sont utilisées, la recherche binaire ne fonctionnera pas.

Dix structures de données importantes : tableau, liste chaînée, pile, file d'attente, table de hachage, arbre binaire, tas, liste de sauts, graphe, arbre Trie.

Dix algorithmes importants : récursivité, tri, recherche binaire, recherche, algorithme de hachage, algorithme glouton, algorithme de diviser pour régner, algorithme de retour en arrière, programmation dynamique, algorithme de correspondance de chaînes.

Pour les structures de données et les algorithmes, il est important non seulement de comprendre ce qu'ils sont, mais surtout de connaître : **leur origine**, **leurs caractéristiques**, **les problèmes qu'ils résolvent**, **les scénarios d'application**.

* [Analyse de la complexité](complexity.md)
  * [Complexité temporelle](complexity.md#shi-jian-fu-za-du)
  * [Complexité spatiale](complexity.md#kong-jian-fu-za-du)
* [Structure de liste linéaire](list.md)
  * [Tableau](list.md#array)
  * [Liste](list.md#list)
  * [Pile](list.md#stack)
  * [File d'attente](list.md#queue)
* [Tri](sort.md)
* [Recherche](search.md)
* [Liste de sauts](skip-list.md)
* [Table de hachage](hash-table.md)
* [Arbre](tree.md)
  * [Arbre binaire de recherche](tree.md#er-cha-cha-zhao-shu)
  * [Arbre binaire de recherche équilibré](tree.md#ping-heng-er-cha-cha-zhao-shu)
  * [Arbre rouge-noir](tree.md#hong-hei-shu)
  * [Arbre récursif](tree.md#di-gui-shu)
  * [Tas](tree.md#dui)
  * Union-Find
  * [Arbre B](tree.md#b-shu)
  * [Arbre B+](tree.md#b-shu-1)
* [Graphe](graph.md)
* [Algorithme de correspondance de chaînes](string-matching.md)
* [Filtre de Bloom](bloom-filter.md)
* Philosophie algorithmique :
  * [Algorithme glouton](greedy-algorithm.md)
  * [Algorithme de diviser pour régner](divide-and-conquer.md)
  * [Algorithme de retour en arrière](back-tracking.md)
  * [Programmation dynamique](dynamic-programming.md)

Comparaison des quatre types d'algorithmes :

* La plupart des problèmes résolus par l'algorithme de diviser pour régner ne peuvent pas être abstraits en problèmes de décision à plusieurs étapes ; les problèmes résolus par la méthode gloutonne, le retour en arrière et la programmation dynamique peuvent tous être abstraits en problèmes de décision à plusieurs étapes.
* L'algorithme de retour en arrière est un "remède universel", les problèmes pouvant être résolus par la programmation dynamique et l'approche gloutonne peuvent généralement être résolus par retour en arrière, qui équivaut à une énumération exhaustive, avec une complexité temporelle très élevée.
* Les problèmes résolus par la programmation dynamique doivent satisfaire les trois caractéristiques suivantes : sous-structure optimale, pas d'effets secondaires, problèmes de sous-structure répétés ; la raison pour laquelle la programmation dynamique est efficace est qu'il y a beaucoup de problèmes de sous-structure répétés dans l'algorithme de retour en arrière, tandis que la méthode de diviser pour régner est l'inverse, nécessitant que les sous-problèmes ne se répètent pas.
* L'algorithme glouton est essentiellement une situation particulière de la programmation dynamique, pour résoudre un problème, il doit satisfaire la structure optimale, pas d'effets secondaires, une sélection avide, sans insister sur les problèmes répétés.
