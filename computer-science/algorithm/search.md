# Recherche binaire

**Description** : La recherche binaire s'applique aux ensembles de données triés. À chaque étape, elle compare l'élément médian de l'intervalle avec l'élément recherché, réduisant ainsi l'intervalle de recherche de moitié jusqu'à ce qu'elle trouve l'élément ou que l'intervalle devienne vide.

**Complexité temporelle** : O(log n), où O(log n) est extrêmement efficace, **parfois même plus efficace que le temps constant**.

**Points clés du code** :

- Condition de sortie de la boucle : `low <= high`
- Valeur de mid : `mid = (low + high) / 2` peut causer un débordement, remplacer par `low + ((high - low) >> 1)`
- `low = mid + 1, high = mid - 1`, au lieu de `low = mid, high = mid`, car cela entraînerait une boucle infinie.

**Limitations** :

- Dépend de la structure de tableau ordonné car nécessite un **accès aléatoire** par index.
- Nécessite des données **triées**. Si non trié, utile pour une recherche unique après un tri initial.
- Pas nécessaire pour une petite quantité de données, une recherche séquentielle est préférable. Exception : si la comparaison des données est coûteuse, comme des chaînes de longueur 300, même pour une petite quantité de données, la recherche binaire est justifiée.
- Pas adapté pour une grande quantité de données en raison de l'exigence de mémoire continue.

{% hint style="info" %}
Dans la plupart des cas, les problèmes résolus avec la recherche binaire peuvent également être résolus avec des tables de hachage et des arbres binaires, mais ces derniers nécessitent une mémoire supplémentaire.
{% endhint %}

La situation la **plus simple** pour la recherche binaire est dans un tableau trié où **les éléments ne sont pas répétés**. Cependant, cela devient légèrement plus complexe lorsque des données répétées sont présentes. Variantes de la recherche binaire :

**Rechercher le premier élément égal à une valeur donnée :**

```java
public static int search(int[] nums, int target) {
    if (nums == null) {
        return -1;
    }
    
    int low = 0, high = nums.length - 1;
    while (low <= high) {
        int mid = low + ((high - low) >> 1);
        if (nums[mid] > target) {
            high = mid - 1;
        } else if (nums[mid] < target) {
            low = mid + 1;
        } else {
            if (mid == 0 || nums[mid - 1] < target) {
                return mid;
            } else {
                high = mid - 1;
            }
        }
    }
    return -1;
}
```

**Rechercher le dernier élément égal à une valeur donnée :**

**Rechercher le premier élément plus grand ou égal à une valeur donnée :**

**Rechercher le dernier élément plus petit ou égal à une valeur donnée :**

{% hint style="info" %}
Pour les requêtes d'égalité, les tables de hachage et les arbres binaires peuvent également résoudre le problème de la recherche binaire, mais pour les "requêtes approximatives" (comme les quatre variantes ci-dessus), l'avantage de la recherche binaire est évident.
{% endhint %}

## Exemples de problèmes

* [LeetCode 69 : Calcul de la racine carrée.](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/search/LT69.java)