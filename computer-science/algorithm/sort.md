# Tri

## Préface

| Algorithme | En place | Stable | Moyen | Meilleur | Pire | Comparaison basée |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Bulle | Oui | Oui | O\(n^2\) | O\(n\) | O\(n^2\) | Oui |
| Insertion | Oui | Oui | O\(n^2\) | O\(n\) | O\(n^2\) | Oui |
| Sélection | Oui | Non | O\(n^2\) | O\(n^2\) | O\(n^2\) | Oui |
| Fusion | Non: O\(n\) | Oui | O\(nlogn\) | O\(nlogn\) | O\(nlogn\) | Oui |
| Rapide | Oui | Non | O\(nlogn\) | O\(nlogn\) | O\(n^2\) | Oui |
| Tas | Oui | Non |  O\(nlogn\) | O\(nlogn\) | O\(nlogn\) | Oui |
| Compte | Non | Oui |  | O\(n+k\), k plage de données |  | Non |
| Seau | Non | Oui |  | O\(n\) |  | Non |
| Radix | Non | Oui |  | O\(dn\), d chiffres |  | Non |

**Dimensions d'évaluation des algorithmes de tri**:

1. Complexité temporelle : meilleur cas, pire cas, moyen, et les types de données correspondants.
2. Coefficients, constantes et termes de plus bas ordre de la complexité temporelle. La complexité temporelle reflète la tendance de croissance lorsque la taille des données n augmente, généralement dans des cas où n est grand. Cependant, dans la pratique, n peut ne pas être très grand, donc sous le même ordre de complexité temporelle, il est nécessaire de prendre en compte les coefficients, les constantes et les termes de plus bas ordre.
3. Analyse du nombre de comparaisons et de mouvements.
4. Consommation de mémoire. **Tri sur place** (trié sur place) signifie une complexité spatiale de O\(1\) pour l'algorithme de tri.
5. Stabilité : si la séquence initiale conserve son ordre relatif après le tri. En pratique, nous trions en fonction d'un champ d'objet, et les objets ont d'autres champs, donc la stabilité doit être analysée. Exemple : commandes avec heure et montant, triées par montant, puis triées par un algorithme de tri stable par heure, le résultat sera montant croissant, montant identique dans l'ordre chronologique.

## Tri à bulles

**Description** : Le tri à bulles ne traite que les éléments adjacents, compare les deux éléments adjacents et, s'ils ne sont pas dans l'ordre, les échange. Chaque passage de bulle déplace un élément à sa position appropriée, en répétant cela n fois, les données de n sont triées.

**Optimisation** : Lorsqu'un passage de bulle n'effectue aucun échange de données, le tri est terminé.

```java
public void tri(Integer[] nums) {
    int n = nums.length;
    for (int i = 0; i < n; i++) {
        boolean échangé = false;
        for (int j = 1; j < n - i; j++) {
            if (nums[j - 1] > nums[j]) {
                échanger(nums, j - 1, j);
                échangé = true;
            }
        }
        if (!échangé) {
            break;
        }
    }
}
private void échanger(Integer[] nums, int i, int j) {
    int tmp = nums[i];
    nums[i] = nums[j];
    nums[j] = tmp;
}
```

**Tri sur place** : Nécessite seulement une mémoire supplémentaire de niveau constant.

**Tri stable** : Lorsque deux éléments adjacents sont égaux, aucun échange n'est nécessaire.

**Meilleur cas en complexité temporelle** : Si les données sont déjà triées, un seul passage de bulle est nécessaire, donc meilleur cas O\(n\).

**Pire cas en complexité temporelle** : Si les données sont exactement inversées, n passages de bulle sont nécessaires, donc pire cas O\(n^2\).

**Complexité temporelle moyenne** :

Le degré d'ordonnancement est le nombre de paires d'éléments qui ont une relation d'ordre dans le tableau, défini comme suit : `a[i] <= a[j], i < j`. Par exemple, dans `2, 4, 3, 1, 5, 6`, le degré d'ordonnancement est `4 + 2 + 2 + 2 + 1 = 11`. Le degré d'ordonnancement complet du tableau trié est appelé degré d'ordonnancement total (\(n * (n - 1) / 2\)). `Désordre = degré d'ordonnancement total - degré d'ordonn

ancement`.

Le tri à bulles a deux opérations de comparaison et d'échange, chaque échange augmente le degré d'ordonnancement de 1, donc `nombre d'échanges = désordre`. Le nombre moyen d'échanges est `n * (n - 1) / 4`, le nombre de comparaisons est supérieur au nombre d'échanges, la limite supérieure de la complexité temporelle est O\(n^2\), donc la complexité temporelle moyenne est O\(n^2\).

Veuillez noter que cette analyse de la complexité temporelle moyenne n'est pas stricte.

## Tri par insertion

**Description** : Divisez le tableau en deux parties, triées et non triées, initialement triées avec le premier élément. À chaque itération, prenez le premier élément de la partie non triée, trouvez sa position appropriée dans la partie triée et insérez-le.

```java
public void tri(Integer[] nums) {
    int n = nums.length;
    for (int i = 1; i < n; i++) {
        int tmp = nums[i];
        int j;
        for (j = i - 1; j >= 0; j--) {
            if (tmp < nums[j]) {
                nums[j + 1] = nums[j];
            } else {
                break;
            }
        }
        nums[j + 1] = tmp;
    }
}
```

**Caractéristiques** : Comprend les opérations de comparaison et de déplacement. Le nombre de déplacements est égal au désordre.

**Tri sur place** : Aucun espace de stockage supplémentaire requis.

**Tri stable** : Lorsque deux éléments sont égaux, en insérant l'élément postérieur après l'élément précédent, l'ordre est préservé.

**Meilleur cas en complexité temporelle** : Si les données sont déjà triées, aucun déplacement de données n'est nécessaire lors de la recherche de la position d'insertion, la recherche de la position d'insertion de chaque élément ne nécessite qu'une seule comparaison, donc le meilleur cas est O\(n\).

**Pire cas en complexité temporelle** : Si les données sont exactement inversées, chaque élément doit être déplacé jusqu'à la première position, donc le pire cas est O\(n^2\).

**Complexité temporelle moyenne** : Pour insérer un élément dans un tableau trié, la complexité temporelle moyenne est O\(n\), donc la complexité temporelle moyenne du tri par insertion est O\(n^2\).

**Pourquoi le tri par insertion est-il meilleur que le tri à bulles ?** : Le nombre d'échanges ou de déplacements est fixe pour le tri à bulles et le tri par insertion, égal au désordre. Mais chaque échange de tri à bulles nécessite trois opérations, tandis que chaque déplacement de tri par insertion n'en nécessite qu'une seule.

**Optimisation du tri par insertion** : Tri de Shell, commencez par de grands écarts, le dernier tri est un tri d'insertion ordinaire, mais la séquence est déjà presque triée. En utilisant de grands écarts, vous pouvez déplacer des positions plus importantes à chaque fois.

## Tri par sélection

**Description** : Divisez le tableau en deux parties, triées et non triées, initialement triées vides. À chaque itération, recherchez l'élément le plus petit dans la partie non triée et placez-le à la fin de la partie triée.

```java
public void tri(Integer[] nums) {
    int n = nums.length;
    for (int i = 0; i < n; i++) {
        int min = nums[i];
        int minIndex = i;
        for (int j = i + 1; j < n; j++) {
            if (nums[j] < min) {
                min = nums[j];
                minIndex = j;
            }
        }
        int tmp = nums[i];
        nums[i] = min;
        nums[minIndex] = tmp;
    }
}
```

**Tri sur place** : Aucun espace de stockage supplémentaire requis.

**Meilleur, pire, et complexité temporelle moyenne** : Tous O\(n^2\).

**Tri instable** : Trouve le plus petit élément dans la partie non triée et l'échange avec le premier élément de la partie non triée, perturbant ainsi l'ordre relatif des éléments égaux.

{% hint style="info" %}
Les tris à bulles et par sélection ne sont utilisés que pour des raisons théoriques, et sont rarement utilisés en pratique, tandis que le tri par insertion est parfois utilisé.
{% endhint %}

{% hint style="info" %}
Si implémenté avec des listes chaînées, l'échange du tri à bulles devient plus complexe ; le tri par insertion n'a pas besoin de déplacements, mais une insertion directe ; l'échange du tri par sélection est également plus complexe.
{% endhint %}

## Tri fusion

**Description** : Utilise une approche de division et conquête. Divisez d'abord le tableau en deux moitiés, puis triez les deux moitiés de manière récursive, puis fusionnez les deux moitiés triées.

```java
public void tri(Integer[] nums) {
    triFusion(nums, 0, nums.length - 1);
}

private void triFusion(Integer[] nums, int p, int r) {
    if (p >= r) {
        return;
    }
    
    int q = (p + r) / 2;
    triFusion(nums, p, q);
    triFusion(nums, q + 1, r);
    fusionner(nums, p, r);
}

private void fusionner(Integer[] nums, int p, int r) {
    int q = (p + r) / 2;
    
    int[] tmp = new int[r - p + 1];
    
    int i = p, j = q + 1, cur = 0;
    while (i <= q && j <= r) {
        if (nums[i] <= nums[j]) {
            tmp[cur++] = nums[i++];
        } else {
            tmp[cur++] = nums[j++];
        }
    }
    
    for (; i <= q; i++) {
        tmp[cur++] = nums[i];
    }
    
    for (; j <= r; j++) {
        tmp[cur++] = nums[j];
    }
    
    cur = 0;
    for (int k = p; k <= r; k++) {
        nums[k] = tmp[cur++];
    }
}
```

**Formule récursive** : `triFusion(p, r) = fusion(triFusion(p, q), triFusion

(q + 1, r))`

**Condition d'arrêt** : `p >= r` ; où `q = (p + r) / 2`.

**Tri stable** : Il dépend de la fusion. Pendant la fusion, en plaçant d'abord les éléments de A\[p, q\] dans un tableau temporaire, l'ordre des éléments égaux est préservé, donc le tri de fusion est stable.

**Meilleur, pire, et complexité temporelle moyenne** : Tous O\(nlogn\), le processus d'analyse est le suivant :

```text
// En supposant que trier n éléments prend T(n) de temps, alors trier deux sous-tableaux prendrait 2 * T(n / 2), 
// et la fusion prendrait un temps linéaire O(n), donc la formule de récurrence est la suivante
T(1) = C
T(n) = 2 * T(n / 2) + K
```

Où K est le temps nécessaire pour fusionner deux sous-problèmes, `K = O(n)`. Ainsi, la complexité temporelle de fusion est O\(nlogn\), indépendamment du degré d'ordonnancement des données d'origine.

**Non tri sur place** : L'opération de fusion nécessite un tableau temporaire, au plus n, donc la complexité spatiale du tri de fusion est O\(n\).

## Tri rapide

**Description** : Le tri rapide suit également le principe de la division et conquête, triant A\[p, r\], en choisissant un élément pivot A\[n\] entre p et r, plaçant les éléments plus petits que le pivot à gauche et les éléments plus grands à droite, puis triant récursivement les deux partitions.

```java
public void tri(Integer[] nums) {
    _tri(nums, 0, nums.length - 1);
}

private void _tri(Integer[] nums, int p, int q) {
    if (p >= q) {
        return;
    }
    
    int r = partitionner(nums, p, q);
    _tri(nums, p, r - 1);
    _tri(nums, r + 1, q);
}

// La fonction de partitionnement de QuickSort est similaire à celle de SelectionSort : diviser le tableau en deux parties, initialement non traitées,
// à chaque fois qu'un élément non traité est sélectionné et comparé au pivot, si c'est plus petit que le pivot, alors il est placé à la fin de la partie traitée, 
// c'est-à-dire échangé avec le premier élément non traité.
private int partitionner(Integer[] nums, int p, int q) {
    int i = p;
    int pivot = nums[q];
    for (int j = p; j < q; j++) {
        if (nums[j] < pivot) {
            int tmp = nums[j];
            nums[j] = nums[i];
            nums[i++] = tmp;
        }
    }
    nums[q] = nums[i];
    nums[i] = pivot;
    return i;
}
```

**Formule récursive** : `triRapide(p, r) = triRapide(p, q - 1) + triRapide(q + 1, r)`

**Condition d'arrêt** : `p >= r`

**Tri instable** : Le tri rapide implique des échanges, donc il est instable.

**Tri sur place** : La fonction de partitionnement ne nécessite aucun espace supplémentaire.

**Analyse de la complexité temporelle** : Dans la plupart des cas, c'est O\(nlogn\), la probabilité de O\(n^2\) est très faible, un pivot raisonnable peut être choisi pour réduire la probabilité. Le pire des cas est lorsque le tableau est déjà trié, en choisissant le dernier élément comme pivot, alors la complexité temporelle est O\(n^2\).

```text
T(1) = C
T(n) = 2 * T(n / 2) + n
```

**Différence entre le tri fusion et le tri rapide** : La fusion est de bas en haut, traitant d'abord les sous-problèmes, puis les fusionnant ; le rapide est de haut en bas, traitant d'abord la partition, puis les sous-problèmes.

### Trouver le k-ième plus grand élément dans un tableau non trié

**Idée** : Divisez le tableau A\[0:n-1\] en tant que dernier élément A\[n-1\] comme pivot, partitionnez en A\[0:p-1\], A\[p\], A\[p+1:n-1\], si p+1=K, alors A\[p\] est le résultat, sinon répétez le processus comme décrit ci-dessus.

**Complexité temporelle** : n + n / 2 + n / 4 + n / 8 + ... + 1 = 2n - 1, donc O\(n\).

## Tri linéaire

La complexité temporelle du tri linéaire est linéaire O\(n\): tri par décompte, tri par base, tri de boîtes. Pouvoir réaliser une complexité temporelle linéaire signifie que l'algorithme ne repose pas sur la comparaison entre éléments et ne nécessite pas de trier en place, ce qui signifie qu'il a une application spécifique.

Le tri linéaire a des exigences très strictes en matière de données et a chacun ses propres scénarios d'application.

### Tri par décompte

**Description** : Si vous devez trier n éléments mais qu'il n'y a que k valeurs possibles, vous pouvez diviser les données en k boîtes, chaque boîte contenant le

 nombre de données correspondant, puis traverser les boîtes pour récupérer les données triées.

**Caractéristiques** : Il peut trier des données entières, mais pas des données flottantes.

**Tri stable** : Si vous souhaitez trier une grande quantité de données avec peu de valeurs possibles, le tri par décompte est le meilleur choix. Mais le tri par décompte n'est pas adapté aux données dispersées.

**Complexité temporelle** : O\(n + k\), où k est la plage de données.

### Tri de base

**Description** : Utilisez des boîtes de données en fonction de la valeur du chiffre bas, triez chaque boîte, puis combinez les résultats.

**Tri stable** : Si vous devez trier une grande quantité de données dans la plage [0, 999], trier d'abord par les unités, puis par les dizaines, puis par les centaines. Tri stable, mais la complexité temporelle est élevée.

**Complexité temporelle** : O\(d \(n + k\)\), d est le nombre de chiffres.

### Tri de boîtes

**Description** : Divisez l'espace des valeurs en petites boîtes, placez chaque valeur dans une boîte correspondante, puis récupérez les valeurs dans l'ordre des boîtes.

**Complexité temporelle** : O\(n + m\), où m est le nombre de boîtes.
