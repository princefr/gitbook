# Retour en arrière

La méthode de traitement par retour en arrière est similaire à une énumération. Nous énumérons toutes les solutions possibles et trouvons celles qui répondent à nos attentes. Pour énumérer systématiquement toutes les solutions, éviter les répétitions et les omissions, le processus de résolution du problème est divisé en plusieurs étapes. À chaque étape, il y a plusieurs choix, nous en faisons un au hasard, et si nous constatons que ce choix ne satisfait pas les conditions, nous revenons à l'étape précédente pour faire un autre choix.

Par exemple, la traversée en profondeur utilise la méthode de retour en arrière.

Généralement, l'algorithme de retour en arrière est implémenté à l'aide de la récursivité. L'opération d'élagage est une technique pour améliorer l'efficacité du retour en arrière.

## Exemples

* [LeetCode 20: Générer des combinaisons de parenthèses valides.](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/backtracking/LT22.java)
* [LeetCode 51: Problème des N reines, générer toutes les solutions.](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/backtracking/LT51.java)
* [LeetCode 52: Problème des N reines, générer le nombre de solutions.](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/backtracking/LT52.java)
* [LeetCode 37: Solutions pour Sudoku.](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/backtracking/LT37.java)
* [LeetCode 79: Déterminer si un mot existe dans un tableau de caractères en deux dimensions.](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/backtracking/LT79.java)
* [LeetCode 212: Trouver des mots existants dans un tableau de caractères en deux dimensions.](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/backtracking/LT212.java)

### Sac à dos 0-1

Problème : un sac à dos a une capacité totale de W kg, et il y a n objets de poids différents. Sélectionnez quelques objets à mettre dans le sac à dos pour maximiser le poids total du sac.

Remarque : ce problème est différent du problème du sac à dos dans l'algorithme glouton, car les objets ici ne peuvent pas être fractionnés, ils doivent être choisis ou non, donc on l'appelle problème 0-1. Ce type de problème est plus efficacement résolu par la [programmation dynamique](dynamic-programming.md).

Idée : Si nous utilisons la méthode de retour en arrière, avec n objets, le problème est divisé en n étapes, chaque étape correspond à choisir ou non un objet. Nous pouvons utiliser quelques astuces d'élagage, comme arrêter les étapes suivantes lorsque le poids des objets choisis dépasse déjà W.

```java
private int maxW = Integer.MIN_VALUE; // stocker le poids total maximum des objets dans le sac à dos
private int[] poids = {2, 2, 4, 6, 3}; // poids de chaque objet
private int n = 5; // nombre d'objets
private int w = 9; // poids du sac à dos
// cw représente le poids total des objets déjà mis dans le sac ; i représente quel objet est examiné ;
// appel : f(0, 0)
public void f(int i, int cw) {
  if (cw == w || i == n) { // cw==w signifie que le sac est plein ; i==n signifie que tous les objets ont été examinés
    if (cw > maxW) maxW = cw;
    return;
  }
  f(i+1, cw); // ne pas choisir l'objet de cette étape
  if (cw + poids[i] <= w) { // si le poids des objets choisis dépasse déjà W, ne pas les choisir
    f(i+1, cw + poids[i]); // choisir l'objet de cette étape
  }
}
```

### Expression régulière