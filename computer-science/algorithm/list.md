# Liste linéaire

Une liste linéaire est une structure de données où les éléments sont disposés de manière linéaire, avec un accès bidirectionnel. Les tableaux, les listes chaînées, les files et les piles sont des exemples de structures de liste linéaire. En revanche, les arbres binaires, les tas et les graphes sont des exemples de structures de données non linéaires.

## Tableau

### Concept

Un tableau est une structure de liste linéaire qui stocke un ensemble de données de même type dans un bloc de mémoire **continu**.

La mémoire continue et le type de données homogène permettent au tableau d'avoir des caractéristiques d'**accès aléatoire**.

**Idée fausse** : La complexité temporelle de la recherche dans un tableau est O(1). En réalité, même pour un tableau trié, la complexité de la recherche est O(log n). La bonne affirmation est que les tableaux prennent en charge l'accès aléatoire, ce qui signifie que l'accès par index a une complexité temporelle de O(1).

### Insertion

L'insertion d'une donnée à la position k du tableau a une complexité temporelle meilleure de O(1), pire de O(n), et en moyenne de O(n).

**Optimisation** : Si le tableau est trié, il est nécessaire de déplacer les éléments ; cependant, s'il est désordonné, il suffit de déplacer les données à la position d'insertion vers la fin, ce qui réduit la complexité temporelle à O(1). C'est le principe utilisé dans le tri rapide.

### Suppression

La suppression des données à la k-ième position a une complexité temporelle meilleure de O(1), pire de O(n), et en moyenne de O(n).

**Optimisation** : Si plusieurs suppressions sont nécessaires, elles peuvent être regroupées pour réduire le nombre de déplacements de données. Il est également possible de marquer les données comme supprimées sans effectuer réellement la suppression, et ne réaliser les suppressions physiques que lorsque l'espace de données est épuisé, ce qui est utilisé dans l'algorithme de collecte des déchets par balayage de la JVM.

### Conteneurs et Tableaux

ArrayList en Java, vector en C++, etc., sont des conteneurs de tableaux.

**Avantages des conteneurs** :

- Encapsulation de nombreuses opérations sur les tableaux, telles que l'insertion et la suppression de données.
- Prise en charge de l'extension dynamique, comme ArrayList en Java, qui se redimensionne automatiquement lorsque sa capacité est insuffisante. Notez que même avec ArrayList, si la taille des données est connue à l'avance, la taille doit être spécifiée lors de l'initialisation pour réduire le nombre d'opérations de demande de mémoire et de déplacement de données.

**Avantages des tableaux** :

- Prise en charge des types de base, tels que int, long en Java, sans nécessiter de classes enveloppantes avec une surcharge de performances.
- Plus d'intuitivité pour les tableaux multidimensionnels, par exemple, Object[][] est plus simple que ArrayList<ArrayList<Object>>.

**Conclusion** :

- Pour le développement d'applications métier, les conteneurs suffisent.
- Pour le développement de bas niveau, les tableaux sont préférables aux conteneurs.

### Numérotation des tableaux à partir de 0

La plupart des langages de programmation numérotent les tableaux à partir de 0. Les raisons en sont :

1. La formule d'adressage du tableau commence par 0, ce qui évite une instruction de soustraction CPU supplémentaire si elle commence à 1.
   
```text
# À partir de 0
a[k]_adress = base_adress + k * type_size

# À partir de 1
a[k]_adress = base_adress + (k -1) * type_size
```

2. Pour des raisons historiques, le langage C a été conçu avec un indexage à partir de 0, et les langages ultérieurs ont suivi cette convention pour réduire les coûts d'apprentissage. Matlab commence à 1, Python prend en charge les index négatifs.

### Exemples de problèmes

* [LeetCode 169 : Trouver l'élément majoritaire dans un tableau](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/array/LT169.java)
* [LeetCode 42 : Trouver le plus petit entier positif manquant dans un tableau](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/array/LT42.java)

## Liste

Comparaison entre les tableaux et les listes : en termes de structure de stockage, les tableaux nécessitent une mémoire continue, ce qui peut entraîner des erreurs de mémoire insuffisante et ne peut pas être étendue dynamiquement. Les listes, en revanche, n'ont pas ce problème et prennent naturellement en charge l'extension dynamique, mais des insertions et suppressions fréquentes peuvent entraîner une fragmentation de la mémoire.

### Types de listes

- **Simple chaînée**, **double chaînée**, **liste circulaire**.

**Simple chaînée** : les éléments sont liés ensemble par des pointeurs. Chaque élément, ou nœud, contient des données et un pointeur vers le prochain élément. Le premier élément est appelé **tête**, qui enregistre l'adresse de base de la liste ; le dernier élément est appelé **queue**, dont le pointeur pointe vers NULL.

**Complexité temporelle** : l'insertion et la suppression dans une liste chaînée sont O(1), mais il faut souvent trouver l'emplacement de suppression, ce qui est O(n), donc la complexité totale pour supprimer une valeur donnée est O(n). L'accès aléatoire au k-ième élément a une complexité temporelle moyenne de O(n).

**Liste circulaire** : la queue pointe vers la tête. Comparée à la liste simple, elle est plus pratique pour parcourir de la fin au début lorsqu'elle est sous forme circulaire.

**Double chaînée** :

- Chaque nœud a un pointeur vers le nœud précédent et suivant.
- Elle prend plus d'espace que la liste simple mais permet une itération bidirectionnelle.
- Supprimer un nœud donné a une complexité de O(1) car il est possible d'accéder directement

 au nœud précédent.

Pour une **liste ordonnée**, la liste double offre une efficacité supérieure à la liste simple. Elle enregistre la position de la dernière recherche, ce qui permet de décider si la recherche suivante doit se faire vers l'avant ou l'arrière.

**Double chaîne circulaire** : une combinaison de liste double et de liste circulaire.

**Stratégies de remplacement de cache** :

- FIFO (First In, First Out)
- LFU (Least Frequently Used)
- LRU (Least Recently Used)

**Implémentation de LRU** : Maintenir une liste chaînée ordonnée. Lorsqu'un nouvel élément est accédé, parcourir la liste pour vérifier s'il est présent. Si c'est le cas, le déplacer au début de la liste. Sinon, s'il y a de la place, insérer le nouvel élément au début. Sinon, supprimer le dernier élément de la liste et insérer le nouveau.

**Astuces de code pour les listes** :

- Utiliser un **sentinelle** pour simplifier les opérations, en particulier pour l'insertion du premier nœud et la suppression du dernier nœud.
- Être attentif aux conditions limites telles que la liste vide, la liste avec un seul nœud, la liste avec deux nœuds. Assurez-vous que les opérations sur les têtes et les queues fonctionnent correctement.
- Utiliser des diagrammes pour aider à la réflexion.

### Exemples de problèmes

* [LeetCode 206 : Inverser une liste](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/list/LT206.java)
* [LeetCode 24 : Inverser les paires d'éléments dans une liste](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/list/LT24.java)
* [LeetCode 25 : Inverser les k éléments d'une liste](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/list/LT25.java)
* [LeetCode 141 : Vérifier si une liste est cyclique](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/list/LT141.java)
* [LeetCode 142 : Trouver le début du cycle dans une liste](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/list/LT142.java)
* [LeetCode 23 : Fusionner k listes triées](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/list/LT23.java)

### Suppression d'éléments en double dans une liste non triée

```java
/**
 * Utiliser un HashSet pour stocker toutes les valeurs
 */
private static void bySet(LNode head) {
    if (head == null) {
        return;
    }
    Set<Integer> values = new HashSet<>();
    LNode pre = head, cur = head.next;
    while (cur != null) {
        if (values.contains(cur.data)) {
            pre.next = cur.next;
        } else {
            values.add(cur.data);
            pre = cur;
        }
        cur = cur.next;
    }
}

/**
 * Implémentation à deux boucles, complexité O(n^2)
 */
private static void byTwoLoop(LNode head) {
    if (head == null) {
        return;
    }
    LNode cur = head.next;
    while (cur != null) {
        Integer data = cur.data;
        int count = 0;
        LNode innerCur = head.next, pre = head;
        while (innerCur != null) {
            if (Objects.equals(innerCur.data, data)) {
                count++;
            }
            if (count > 1) {
                pre.next = innerCur.next;
                count--;
            } else {
                pre = innerCur;
            }
            innerCur = innerCur.next;
        }
        cur = cur.next;
    }
}
```

## Pile

**Définition** : Une pile est une structure de liste linéaire dont le principe est le LIFO (dernier entré, premier sorti) ou FILO (premier entré, dernier sorti).

**Opérations** : Push pour empiler, Pop pour dépiler.

**Types** : Implémentation basée sur un tableau appelée **pile séquentielle**, et sur une liste chaînée appelée **pile chaînée**.

**Complexité spatiale** : O(1), car l'espace supplémentaire nécessaire pour stocker les données est constant, indépendamment de la taille des données.

**Complexité temporelle** : O(1) pour Push et Pop.

**Pile avec extension dynamique** : Temps constant pour Pop, O(1) pour Push dans le meilleur des cas, O(n) dans le pire des cas, et O(1) en moyenne. Cependant, ce type de pile est rarement rencontré.

**Applications de la pile** :

- **Pile d'appels de fonctions**, où chaque appel de fonction ajoute une trame de pile, et chaque retour de fonction la retire.
- **Évaluation d'expressions**, parcourant l'expression de gauche à droite et en utilisant une pile pour stocker les opérandes et les opérateurs. Quand un opérateur est rencontré, il est appliqué aux deux opérandes du haut de la pile.
- **Vérification de l'équilibre des parenthèses**, où une pile est utilisée pour vérifier que chaque parenthèse d'ouverture a une parenthèse de fermeture correspondante.
- **Fonctionnalité de navigation avant/arrière dans les navigateurs**, où deux piles sont utilisées pour stocker les URL visitées.

### Exemples de problèmes

* [LeetCode 20 : Vérifier si une chaîne de parenthèses est équilibrée](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/stack/LT20.java)
* [LeetCode 232 : Implémenter une file avec des piles](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/stack/LT232.java)
* [LeetCode 225 : Implémenter une pile avec des files](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/stack/LT225.java)
* [LeetCode 844 : Vérifier si deux séquences d'édition produisent la même chaîne](https://github.com/StoneYunZhao/algorithm/blob/master/src/main

/java/com/zhaoyun/leetcode/stack/LT844.java)

## File

**Définition** : Une file est une structure de liste linéaire où les éléments sont traités dans l'ordre FIFO (premier entré, premier sorti).

**Opérations** : Enfilez (enqueue) pour ajouter des données à la file, et défiler (dequeue) pour retirer les données de la file.

**Types** : Implémentation basée sur un tableau appelée **file séquentielle**, et sur une liste chaînée appelée **file chaînée**.

**Implémentation** : Une file nécessite deux pointeurs, tête pointant vers le premier élément et queue pointant vers le dernier élément.

- Pour une implémentation basée sur un tableau, lorsque queue = n, une réorganisation complète des données est effectuée. Les temps d'enfilement et de défiler sont tous deux de O(1).
- Pour une implémentation basée sur une liste chaînée, enfilez ajoute un nouvel élément à la fin de la liste, et défiler supprime le premier élément de la liste.

**File circulaire** : En définissant le nœud suivant de la queue comme la tête, on évite les opérations de réorganisation des données. La difficulté réside dans la détermination des conditions de file vide (head = queue) et de file pleine ((queue + 1) % n = head). Une file circulaire gaspille un espace de stockage dans le tableau, mais cela peut être évité en augmentant la taille de la file.

**File bloquante** : L'accès à la file est bloqué lorsque la file est vide (défilement bloqué) ou pleine (enfilage bloqué).

**File concurrente** : Une file sécurisée pour les threads. L'implémentation sans verrou utilise le mécanisme CAS (Compare-and-Swap). Avant d'enfiler, obtenez la position de la queue, puis vérifiez si elle a changé avant de procéder à l'enfilage. Pour défiler, obtenez la position de la tête et vérifiez avec CAS.

## File de priorité

Dans une file de priorité, les éléments sont enfilés de manière régulière, mais sont défiler en fonction de leur priorité. Les implémentations typiques sont :

- **Tas**
- **Arbre binaire de recherche**

### Exemples de problèmes

* [LeetCode 703 : Trouver le k-ième plus grand élément dans un flux de données](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/queue/LT703.java)
* [LeetCode 239 : Trouver les maximums glissants d'une fenêtre dans un tableau](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/queue/LT239.java)