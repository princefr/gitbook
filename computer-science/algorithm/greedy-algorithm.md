# Algorithme glouton

## Introduction

L'algorithme glouton a de nombreuses applications classiques, telles que le codage de Huffman (Huffman Coding), les algorithmes de l'arbre couvrant minimal de Prim et de Kruskal, et l'algorithme de plus court chemin de Dijkstra.

Principes de l'algorithme glouton :

1. Pour un ensemble de données donné, nous définissons une **valeur de contrainte** et une **valeur attendue**, avec pour objectif de sélectionner quelques données parmi celles-ci de manière à maximiser la valeur attendue tout en respectant la valeur de contrainte.
2. À chaque étape, nous choisissons les données qui, parmi celles disponibles, contribuent le plus à la valeur attendue tout en consommant la même valeur de contrainte ; ou bien, qui contribuent de manière équivalente à la valeur attendue tout en minimisant la consommation de la valeur de contrainte.
3. Nous examinons quelques exemples pour voir si les résultats produits par l'algorithme glouton sont optimaux. La preuve rigoureuse de la validité de l'algorithme glouton est très complexe.

{% hint style="info" %}
L'utilisation de l'algorithme glouton pour résoudre un problème ne garantit pas toujours la solution optimale. Cela est particulièrement vrai lorsque les choix effectués initialement peuvent influencer les choix ultérieurs.
{% endhint %}

## Exemples

* [LeetCode 122 : Meilleur moment pour acheter et vendre des actions](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/greedy/LT122.java)

### Problème du sac à dos

Problème : Un sac à dos peut contenir 100 kg. Il y a cinq types de haricots, lesquels devraient être mis dans le sac à dos et combien de chaque type pour maximiser la valeur totale du sac à dos.

| Produit | Poids total (kg) | Valeur totale (€) |
| :--- | :--- | :--- |
| Haricots jaunes | 100 | 100 |
| Haricots verts | 30 | 90 |
| Haricots rouges | 60 | 120 |
| Haricots noirs | 20 | 80 |
| Haricots bleus | 50 | 75 |