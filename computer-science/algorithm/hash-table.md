# Table de hachage

**Définition** : Une table de hachage, utilisant les caractéristiques d'accès aléatoire des tableaux, mappe les clés des éléments à des indices de tableau à l'aide d'une fonction de hachage, et stocke les données à la position correspondante du tableau.

Le temps de recherche théorique est en O(1), mais dépend de la fonction de hachage, du facteur de charge, des collisions, etc.

## Fonction de hachage

**Exigences de base** :

1. Le résultat doit être un entier non négatif.
2. Si key1 = key2, alors hash(key1) = hash(key2).
3. Si key1 ≠ key2, alors hash(key1) ≠ hash(key2). // Presque impossible à réaliser, les collisions ne peuvent pas être évitées.

**Principes de conception** :

* Ne doit pas être trop complexe pour ne pas consommer trop de ressources de calcul.
* La valeur générée doit être aussi aléatoire et uniformément distribuée que possible.

**Exemples** :

* Méthode d'analyse des données : par exemple, les premiers chiffres des numéros de téléphone sont souvent répétés, donc les quatre derniers chiffres sont utilisés comme valeur de hachage.
* Méthode du carré moyen.
* Méthode des nombres aléatoires.
* Méthode du pliage.
* Méthode de recherche directe.

## Algorithme de hachage

**Définition** : Un algorithme qui mappe une chaîne binaire de longueur arbitraire à une chaîne binaire de longueur fixe, appelée la valeur de hachage.

**Exigences de l'algorithme de hachage** :

1. On ne doit pas pouvoir retrouver les données d'origine à partir de la valeur de hachage.
2. Sensible aux données d'entrée : modifier un seul bit des données d'origine doit produire une valeur de hachage complètement différente.
3. La probabilité de collision doit être faible, mais ne peut pas être totalement évitée (principe des tiroirs).
4. Haute efficacité d'exécution : doit pouvoir calculer rapidement la valeur de hachage même pour de longs textes.

**Applications de l'algorithme de hachage** :

* **Cryptage sécurisé**, MD5 (Message Digest Algorithm), SHA (Secure Hash Algorithm). Les points 1 et 3 sont très importants.
* **Identifiant unique**, recherche d'images dans une grande bibliothèque, chaque image est hachée, puis les images originales sont comparées.
* **Vérification des données**, division d'un fichier de téléchargement BT en plusieurs blocs, chaque bloc est haché et enregistré dans le fichier de semence pour garantir l'intégrité des données.
* **Fonction de hachage**, met l'accent sur le point 4, les points 1 et 3 ne sont pas très importants.
* **Équilibrage de charge**, implémentation de la session collante, en hachant l'adresse IP du client et en effectuant un modulo sur le nombre de serveurs.

## Collision de hachage

La troisième exigence de la fonction de hachage est presque impossible à réaliser, donc des collisions de hachage peuvent se produire. Deux solutions sont couramment utilisées : le **probing ouvert** (open addressing) et la **chaining**.

### **Probing ouvert**

En cas de collision de hachage, la position est réexaminée et l'élément est inséré à cet endroit.

Quelle que soit la méthode de recherche utilisée, lorsque l'espace libre est faible, la probabilité de collision augmente considérablement. Ceci peut être contrôlé en ajustant le facteur de charge.

**Avantages** :

* Les données sont toutes stockées dans un tableau, ce qui permet une utilisation efficace du cache CPU pour accélérer les recherches.
* La sérialisation est simple.

**Inconvénients** :

* La suppression des données est plus compliquée, car elles doivent être marquées comme supprimées.
* Toutes les données sont dans un tableau, ce qui rend les collisions plus coûteuses.

**Résumé** : Approprié pour un petit nombre de données et un faible facteur de charge. La classe ThreadLocalMap de Java utilise le probing ouvert.

#### **Probing linéaire (Linear Probing)**

Lors de l'insertion de données, si la position est occupée, on continue de chercher jusqu'à trouver une position vide.

Pour la recherche de données, on calcule d'abord la valeur de hachage pour trouver la position correspondante, puis on compare. Si elles ne correspondent pas, on continue de chercher jusqu'à trouver une position vide. Si on trouve une position vide avant de trouver la donnée, cela signifie que la donnée n'existe pas.

Pour supprimer des données, il ne suffit pas de les définir sur null, car cela poserait problème lors de la recherche. Les données doivent donc être marquées comme supprimées.

Ce procédé est problématique, car plus les données sont nombreuses, plus il y a de collisions, et la complexité temporelle devient O(n).

#### **Probing quadratique (Quadratic Probing)**

La longueur du pas de recherche est hash(key) + 0, hash(key) + 1 ^ 2, hash(key) + 2 ^ 2, ……

#### **Double hachage (Double Hashing)**

Le double hachage utilise un ensemble de fonctions de hachage : hash1(key), hash2(key), hash3(key), ......

### **Chaining**

Chaque seau ou emplacement de la table de hachage est associé à une liste, et les éléments ayant la même valeur de hachage sont placés dans la même liste.

**Avantages** :

* Taux d'utilisation de la mémoire élevé, car les nœuds peuvent être créés à la demande.
* Tolérance plus élevée au facteur de charge, même si le facteur de charge est de 10, cela ne ralentira pas trop.

**Inconvénients** :

* Nécessite le stockage des pointeurs, ce qui peut être coûteux en mémoire pour les petits objets.
* Pas favorable au cache CPU.

**Résumé** : Convient pour stocker de gros objets et de grandes quantités de données ; plus flexible, supporte plus de stratégies d'optimisation, comme le remplacement des listes par des arbres rouge-noir.

## Facteur de charge

```text
Facteur de charge de la table de hachage = Nombre d

'éléments dans la table / Longueur de la table de hachage
```

Lorsque le facteur de charge est trop élevé, le redimensionnement dynamique est possible. Le redimensionnement dynamique de la table de hachage nécessite de recalculer la position de stockage de chaque donnée.

Pour une table de hachage redimensionnée de manière dynamique, l'analyse d'amortissement montre que le temps d'insertion est O(1) au mieux, O(n) au pire, et O(1) en moyenne.

Pour une table de hachage redimensionnée dynamiquement, la décision de réduction dépend des données après la suppression.

Méthodes de redimensionnement :

* Redimensionnement en une seule fois : lorsqu'un redimensionnement est nécessaire, toutes les données sont déplacées. Cela rend certaines opérations d'insertion très lentes.
* Redimensionnement par lot : lorsqu'un redimensionnement est nécessaire, seulement de l'espace est alloué. Chaque fois qu'une nouvelle donnée est insérée, elle est insérée dans la nouvelle table de hachage, et une donnée est retirée de l'ancienne table de hachage. Lors de la recherche, les deux tables de hachage sont consultées.

## Table de hachage industrielle

HashMap de Java :

* Taille initiale par défaut de 16, peut être ajustée.
* Facteur de charge maximal par défaut de 0.75, la taille est doublée lors du redimensionnement.
* Collision de hachage, utilise le chaining, depuis JDK1.8, introduit l'arbre rouge-noir. Si la liste dépasse 8 éléments, elle est convertie en arbre rouge-noir. Lorsque le nombre de nœuds de l'arbre rouge-noir est inférieur à 8, il est converti en liste.

```text
int hash(Object key) {
    int h = key.hashCode()；
    return (h ^ (h >>> 16)) & (capacité -1); //capacité représente la taille de la table de hachage
}
```

## Combinaison de table de hachage et de tableau

### **LRU Cache**

L'algorithme LRU basé sur une liste a une complexité temporelle de O(n) (voir [Liste](list.md#list)). En combinant une table de hachage et une liste doublement liée, la complexité temporelle peut être réduite à O(1).

**Implémentation** : Chaque nœud contient quatre données : data pour stocker les données principales, prev et next pour la liste doublement liée, hnext pour la liste de hachage. Il est également nécessaire de maintenir un nœud de queue de liste doublement liée pour une suppression rapide de données.

**Recherche** : Recherche via la table de hachage en O(1), une fois les données trouvées, elles sont déplacées vers le nœud de tête de la liste doublement liée.

**Suppression** : Recherche via la table de hachage en O(1) pour trouver le nœud à supprimer, puis suppression à la fois dans la liste de hachage et dans la liste doublement liée.

**Ajout** : Vérifier d'abord si l'élément existe ; s'il existe, le déplacer vers la tête de liste ; s'il n'existe pas et que la liste n'est pas pleine, l'ajouter en tête ; s'il n'existe pas et que la liste est pleine, supprimer d'abord le nœud de queue de la liste doublement liée, puis ajouter en tête.

### **Ensemble ordonné de Redis**

Dans un ensemble ordonné de Redis, chaque élément a deux attributs : clé et score.

Les API d'un ensemble ordonné Redis sont :

1. Ajout
2. Suppression par clé
3. Recherche par clé
4. Recherche par intervalle de score
5. Tri par score

**Implémentation** : D'abord, un saut est construit en fonction du score, ce qui permet de réaliser rapidement les API 4 et 5. Ensuite, de manière similaire à LRU, une table de hachage est construite en fonction de la clé.

### **LinkedHashMap de Java**

```text
// 10 est la taille initiale, 0.75 est le facteur de charge, true signifie qu'elle est triée par ordre d'accès
HashMap<Integer, Integer> m = new LinkedHashMap<>(10, 0.75f, true);
m.put(3, 11);
m.put(1, 12);
m.put(5, 23);
m.put(2, 22);
​
m.put(3, 26); // remplacera le précédent 3, ce 3 sera déplacé à la fin de la liste doublement liée
m.get(5); // après l'accès à 5, les données accédées seront déplacées à la fin de la liste
​
for (Map.Entry e : m.entrySet()) {
  System.out.println(e.getKey());
}

// sortie : 1 2 3 5
```

LinkedHashMap est triée par ordre d'accès, le mot "Linked" signifie qu'elle utilise une liste doublement liée. Fondamentalement, c'est la même implémentation que LRU.

## Problèmes d'exemple

* [LeetCode 242 : Vérifier si deux chaînes sont des anagrammes.](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/map/LT242.java)
* [LeetCode 1 : Deux sommes.](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/map/LT01.java)
* [LeetCode 15 : Trois sommes.](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/map/LT15.java)
* [LeetCode 36 : Vérifier la validité du problème du sudoku.](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/map/LT36.java)