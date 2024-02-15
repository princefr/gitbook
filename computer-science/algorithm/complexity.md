# Complexité

## Complexité temporelle

Le temps total d'exécution du code `T(n)` est proportionnel au nombre total d'exécutions de toutes les lignes de code `f(n)`, c'est-à-dire :

```text
T(n) = O(f(n))
```

Ici, n représente la taille de l'ensemble de données, et O représente une fonction proportionnelle.

La complexité temporelle en notation Big O ne représente pas le temps réel d'exécution du code, mais plutôt la **tendance de variation du temps d'exécution du code avec l'augmentation de la taille des données**, également appelée **complexité temporelle asymptotique**. 

Lorsque n est grand, les termes de faible ordre, les constantes et les coefficients dans f(n) n'affectent pas la tendance de croissance, ils peuvent donc être ignorés. Par exemple, O(2n^2+2n+3) peut être représenté comme O(n^2). Ainsi, l'analyse de la complexité temporelle comprend quelques techniques pratiques :

1. Se concentrer uniquement sur la ligne de code avec le plus grand nombre d'itérations.
2. Règle d'addition : la complexité totale est égale à la complexité du niveau le plus élevé. Par exemple, si deux segments de code ont des complexités respectives de O(n) et O(n^2) qui s'exécutent l'un après l'autre, alors la complexité totale est O(n^2).
3. Règle de multiplication : la complexité d'un code imbriqué est le produit des complexités des codes intérieurs et extérieurs. Par exemple, si deux segments de code imbriqués ont des complexités respectives de O(n) et O(n^2), alors la complexité totale est O(n^3).

Quelques complexités temporelles courantes :

```text
# Ordres polynomiaux
O(1) // Constante
O(logn) // Logarithmique
O(n) // Linéaire
O(nlogn) // Linéaire-logarithmique, comme dans les tris fusion et rapide
O(n^2) // Quadratique
O(n^3) // Cubique
O(n^k) // Puissance k
​
# Ordres non polynomiaux
O(2^n) // Exponentiel
O(n!) // Factoriel
```

Les problèmes algorithmiques avec une complexité non polynomiale sont appelés **problèmes NP** (Non-Deterministic Polynomial).

Lorsqu'un segment de code a une complexité déterminée par deux tailles de données et qu'on ne sait pas laquelle des deux est la plus grande, alors la complexité obéit à la règle d'addition : O\(f\(m\) + f\(n\)\), et à la règle de multiplication : O\(f\(m\) \* f\(n\)\).

**Complexité dans différents cas**

- Complexité dans le meilleur des cas (best case time complexity)
- Complexité dans le pire des cas (worst case time complexity)
- Complexité moyenne des cas (average case time complexity)

Pour un même bloc de code, la complexité temporelle peut **différer en fonction des situations**, nous devons donc utiliser les trois types de complexité mentionnés ci-dessus. Par exemple, dans le code suivant, lors de la recherche d'un élément dans un tableau, la complexité est de O(1) dans le meilleur des cas, de O(n) dans le pire des cas, et en moyenne de O(n).

```java
int trouver(int[] tableau, int n, int x) {
  int i = 0;
  int pos = -1;
  for (; i < n; ++i) {
    if (tableau[i] == x) {
       pos = i;
       break;
    }
  }
  return pos;
}
```

**Analyse d'amortissement**

Lorsque vous effectuez des opérations continues sur une structure de données, dans la plupart des cas, la complexité temporelle est très faible, sauf dans quelques cas où elle est très élevée. De plus, ces opérations sont liées dans l'ordre, vous pouvez donc analyser ce groupe d'opérations ensemble pour voir si la complexité temporelle élevée peut être étalée sur les opérations à faible complexité temporelle. L'analyse d'amortissement donne alors une complexité temporelle appelée **complexité temporelle amortie** (amortized time complexity).

Fondamentalement, la complexité temporelle amortie est une forme spéciale de complexité temporelle moyenne, généralement, les deux sont égaux.

## Complexité spatiale

La complexité spatiale asymptotique représente la **relation de croissance entre l'espace de stockage de l'algorithme et la taille des données**.

Quelques complexités spatiales courantes : `O(1)`, `O(n)`, `O(n^2)`.