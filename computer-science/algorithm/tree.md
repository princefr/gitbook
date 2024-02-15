# Arbre

## Concepts

**Structure linéaire** : Pile, file  
**Structure non linéaire** : Arbre, graphe

Chaque élément de l'arbre (Tree) est appelé **noeud**, et la connexion entre les noeuds est appelée **relation parent-enfant**. Un noeud parent pointe vers un noeud enfant. Ceux qui ont le même parent sont appelés **noeuds frères**. Celui qui n'a pas de parent est appelé **noeud racine**, et celui qui n'a pas d'enfant est appelé **noeud feuille**.

**Hauteur du noeud** (Height) : La plus longue distance d'un noeud à un noeud feuille (en nombre de bords).  
**Profondeur du noeud** (depth) : Le nombre de bords parcourus depuis le noeud racine jusqu'à ce noeud.  
**Niveau du noeud** (Level) : La profondeur du noeud + 1.  
**Hauteur de l'arbre** : La hauteur du noeud racine.

**Arbre binaire** : Chaque noeud a au plus deux noeuds enfants, appelés respectivement le fils gauche et le fils droit.  
**Arbre binaire complet** : Tous les noeuds feuilles se trouvent au dernier niveau, chaque noeud (sauf les feuilles) a deux enfants.  
**Arbre binaire parfait** : Tous les noeuds feuilles se trouvent aux deux derniers niveaux, les noeuds feuilles du dernier niveau sont disposés à gauche, et chaque niveau, sauf le dernier, a le nombre maximal de noeuds. Un arbre binaire parfait peut être stocké dans un tableau de manière très économique.

**Méthode de stockage chaînée** : Chaque noeud a trois champs : données et deux pointeurs.  
**Méthode de stockage séquentiel** : Stockage dans un tableau, le noeud racine est stocké à l'indice `1`, si le noeud X est à l'indice `i`, alors son fils gauche est à l'indice `2 * i`, son fils droit est à l'indice `2 * i + 1`, et son parent est stocké à l'indice `i / 2`. Un arbre binaire complet gaspille seulement la position `0`, mais un arbre non complet gaspille beaucoup d'espace.

La traversée de l'arbre binaire a une complexité temporelle de `O(n)` :

* Traversée préfixe : Noeud -> Sous-arbre gauche -> Sous-arbre droit
* Traversée infixée : Sous-arbre gauche -> Noeud -> Sous-arbre droit
* Traversée postfixée : Sous-arbre droit -> Sous-arbre gauche -> Noeud

## Arbre binaire de recherche

Un **arbre binaire de recherche** (binary search tree) : Pour chaque noeud, tous les noeuds du sous-arbre gauche sont inférieurs à ce noeud, et tous les noeuds du sous-arbre droit sont supérieurs à ce noeud.

* **Recherche** : Commencez par le noeud racine, si c'est égal, retournez-le, si c'est plus petit, recherchez récursivement dans le sous-arbre gauche, si c'est plus grand, recherchez récursivement dans le sous-arbre droit.
* **Insertion** : Les données insérées sont placées dans les noeuds feuilles. Commencez par le noeud racine, si les données à insérer sont plus grandes et que le sous-arbre droit est vide, insérez-les dans le noeud droit, sinon parcourez récursivement le sous-arbre droit pour trouver l'emplacement d'insertion. Pour les données plus petites, l'insertion est similaire mais du côté gauche.
* **Suppression** : Si le noeud à supprimer n'a pas d'enfants, définissez le pointeur parent de ce noeud sur nul ; si le noeud à supprimer a un seul enfant, définissez le pointeur parent de ce noeud sur l'enfant de ce noeud ; si le noeud à supprimer a deux enfants, trouvez le plus petit noeud dans le sous-arbre droit du noeud à supprimer, remplacez le noeud à supprimer par ce plus petit noeud, puis appliquez les deux premières règles pour supprimer l'emplacement du plus petit noeud d'origine. Vous pouvez également le marquer comme supprimé mais ne pas le supprimer réellement pour économiser de la mémoire.
* Recherche du noeud maximal.
* Recherche du noeud minimal.
* Recherche du prédécesseur.
* Recherche du successeur.
* Sortie de la séquence de données ordonnées, juste en faisant une traversée infixée, avec une complexité temporelle de O(n), c'est pourquoi l'arbre binaire de recherche est également appelé arbre binaire trié.

En pratique, dans un arbre binaire de recherche, ce qui est stocké sont des objets, et un champ de l'objet est utilisé comme clé pour construire l'arbre binaire de recherche, les autres champs de l'objet sont appelés **données satellites**.

**Arbre binaire de recherche avec données répétées** :

* En utilisant une liste chaînée ou une structure de données prenant en charge l'expansion dynamique comme noeud, stockez toutes les données ayant la même valeur dans le même noeud.
* Chaque noeud stocke toujours une donnée, lors de l'insertion de données, si elles sont identiques, insérez-les dans le sous-arbre droit de ce noeud, les traitant comme plus grandes que ce noeud ; lors de la recherche, si vous rencontrez la même valeur, ne vous arrêtez pas, continuez à chercher dans le sous-arbre droit jusqu'à rencontrer un noeud feuille, et trouvez tous les noeuds ayant la même valeur que celle-ci ; lors de la suppression, trouvez chaque noeud à supprimer et supprimez-le séquentiellement.

La **complexité temporelle** de l'arbre binaire de recherche : pire cas `O(n)`, meilleur cas `O(hauteur) = O(logn)`, en moyenne `O(logn)`.

**Tableaux de hachage vs Arbres binaires de recherche** :

* Les données dans une table de hachage sont désordonnées, il est difficile de produire des données triées ; une traversée infixée d'un arbre binaire de recherche suffit.
* L'extension d'une table de hachage prend beaucoup de temps et sa performance est instable ; l'arbre binaire de recherche équilibré a une performance stable.
* Bien que la recherche dans une table de

 hachage soit en O(1), en raison des collisions de hachage, ce n'est pas nécessairement mieux que O(logn).
* L'implémentation d'une table de hachage est plus complexe, nécessitant la conception de la fonction de hachage, la résolution des collisions, l'extension et la réduction, etc.
* Les tables de hachage gaspillent un certain espace.

### Exemples

* [LeetCode 98 : Vérifiez si un arbre est un arbre binaire de recherche.](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/tree/LT98.java)
* [LeetCode 235 : Ancêtre commun d'un arbre binaire de recherche.](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/tree/LT235.java)
* [LeetCode 236 : Ancêtre commun d'un arbre binaire.](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/tree/LT236.java)

## Arbre binaire de recherche équilibré

Pourquoi avons-nous besoin d'un **arbre binaire de recherche équilibré** ? Dans le meilleur des cas, la complexité temporelle des opérations d'insertion, de suppression et de recherche dans un arbre binaire de recherche est de `O(logn)`, mais lorsqu'il est constamment mis à jour de manière dynamique, il peut se dégrader, dans le pire des cas, en une liste chaînée. Pour résoudre ce problème de dégradation de complexité, nous avons besoin d'un arbre binaire de recherche équilibré.

**Arbre binaire équilibré** : Dans un arbre binaire, la différence de hauteur entre les sous-arbres gauche et droit de chaque noeud ne peut pas être supérieure à 1. Les arbres binaires complets et les arbres binaires pleins sont des arbres binaires équilibrés.

**Arbre binaire de recherche équilibré** : Un arbre binaire de recherche qui est également équilibré. Les arbres AVL, les tréaps (heap trees), les arbres Splay sont des arbres binaires de recherche équilibrés stricts. Les arbres rouges-noirs ne le sont pas strictement.

## Arbre rouge-noir

Arbre rouge-noir, abrégé en R-B Tree. **Définition** :

* Le noeud racine est noir.
* Chaque noeud feuille est un noeud NULL noir et ne stocke pas de données.
* Aucun noeud rouge n'a deux noeuds rouges enfants adjacents.
* Pour chaque noeud, tous les chemins de ce noeud à tous les noeuds feuilles ont le même nombre de noeuds noirs.

Voici deux exemples d'arbres rouges-noirs (les noeuds noirs vides sont omis dans l'image) :

![Exemple d'arbres rouges-noirs](../../.gitbook/assets/image%20%2826%29.png)

Analyse des performances temporelles des arbres rouges-noirs :

* De nombreuses opérations sur un arbre binaire de recherche ont une complexité temporelle proportionnelle à la hauteur de l'arbre, donc nous avons juste besoin d'analyser la hauteur de l'arbre rouge-noir.
* En supprimant les noeuds rouges de l'arbre rouge-noir, on obtient un arbre quaternaire.
* Selon la quatrième règle, cet arbre quaternaire a une hauteur inférieure à un arbre binaire complet avec le même nombre de noeuds, c'est-à-dire moins que log2n.
* En réintroduisant les noeuds rouges, selon la troisième règle, la hauteur de l'arbre rouge-noir est inférieure à 2log2n.

{% hint style="info" %}
Les arbres rouges-noirs proviennent des arbres 2-3, et l'on peut comprendre les opérations d'insertion et de suppression des arbres rouges-noirs à partir des arbres 2-3.
{% endhint %}

## Arbre Recursif

### Récursivité

Un problème peut être résolu de manière récursive s'il satisfait aux trois conditions suivantes :

1. Le problème peut être décomposé en plusieurs sous-problèmes.
2. Le problème et ses sous-problèmes sont résolus de la même manière, à l'exception de la taille des données.
3. Il existe une condition d'arrêt pour la récursivité.

La clé pour écrire du code récursif est de définir une formule récursive et de trouver la condition d'arrêt.

Lorsque vous pensez à la récursivité, il ne faut pas essayer de comprendre l'ensemble du processus récursif. La meilleure approche consiste à considérer le problème A comme pouvant être résolu en fonction des sous-problèmes B, C et D. Il suffit alors de réfléchir à la relation entre A et B, C, D sur deux niveaux, sans avoir besoin de descendre niveau par niveau.

* La récursivité doit être prudente pour éviter le débordement de pile. Limiter la profondeur maximale de récursion peut résoudre ce problème.
* La récursivité doit être attentive aux calculs redondants. La mise en cache des résultats déjà calculés peut être utile.
* La récursivité peut entraîner un grand nombre d'appels de fonction. Dans certains cas, il peut être préférable de les remplacer par des approches non récursives.

**Exemple** : Supposons qu'il y ait n marches d'escalier, et à chaque étape, vous pouvez monter 1 ou 2 marches à la fois. Déterminez combien de façons différentes il y a de monter les n marches. Formule récursive : `f(n) = f(n-1) + f(n-2)`. Condition d'arrêt : `f(1) = 1, f(2) = 2`

```python
# Récursif
def f(n):
    if n == 1:
        return 1
    if n == 2:
        return 2
    return f(n-1) + f(n-2)

# Non récursif
def f(n):
    if n == 1:
        return 1
    if n == 2:
        return 2

    ret = 0
    pre = 2
    prepre = 1
    for i in range(3, n+1):
        ret = pre + prepre
        prepre = pre
        pre = ret
    return ret
```

Le processus récursif peut être représenté sous forme de graphique, ce qui donne un **arbre récursif**. L'arbre récursif peut être utilisé pour déterminer la complexité temporelle.

### Complexité temporelle de la fusion

![Arbre récursif de la fusion](../../.gitbook/assets/image%20%28242%29.png)

La fusion triée a principalement des opérations de division et de fusion. Les opérations de division ont un coût très faible, tandis que le coût de la fusion dépend de la taille des données, mais la somme des tailles de données dans chaque niveau de l'arbre récursif est la même. Supposons que chaque niveau coûte n, alors la complexité temporelle de la fusion triée est O(h * n), où h est la hauteur de l'arbre. L'arbre récursif de la fusion triée est un arbre binaire complet, donc sa hauteur est log2n, donc la complexité temporelle de la fusion triée est O(nlogn).

### Complexité temporelle de QuickSort

![Arbre récursif de QuickSort](../../.gitbook/assets/image%20%28167%29.png)

Le tri rapide ne garantit pas que chaque partition soit égale, supposons que chaque partition soit 1:9. La somme du nombre de données parcourues à chaque niveau d'arbre récursif est n, donc il suffit de calculer la hauteur de cet arbre récursif, mais cet arbre n'est pas un arbre binaire complet.

Du nœud racine n au nœud feuille 1, le chemin le plus court est multiplié par 1/10 à chaque fois, le chemin le plus long est multiplié par 9/10 à chaque fois, donc le chemin le plus court est nlog10(n), le chemin le plus long est nlog10/9(n), donc la complexité temporelle de QuickSort est O(nlogn).

### Complexité temporelle de la suite de Fibonacci

![Arbre récursif de la suite de Fibonacci](../../.gitbook/assets/image%20%2822%29.png)

De manière similaire, du nœud racine n au nœud feuille 1, le chemin le plus long est n, le chemin le plus court est n/2. Le coût de chaque niveau est la somme des opérations d'addition pour les nœuds de ce niveau, donc le premier niveau est 1, le deuxième niveau est 2, le n-ème niveau est 2^(n-1). Ensuite, multipliez les chemins, donc le coût est O(2^n).

### Complexité temporelle de la permutation complète

La permutation complète d'une séquence de nombres, si nous fixons le dernier chiffre, il reste n - 1 problèmes de permutation. Le code est :

```python
# a = a={1, 2, 3, 4}; printPermutations(a, 4, 4);
def printPermutations(data, n, k):
    if k == 1:
        for i in range(n):
            print(data[i], end=" ")
        print()
    
    for i in range(k):
        tmp = data[i]
        data[i] = data[k-1]
        data[k-1] = tmp
        
        printPermutations(data, n, k - 1)
        
        tmp = data[i]
        data[i] = data[k-1]
        data[k-1] = tmp
```

L'arbre récursif ci-dessous montre que le premier niveau a n échanges, le deuxième niveau a n noeuds, chaque noeud a n - 1 échanges, et ainsi de suite. Donc, la complexité est O(n!).

![Arbre récursif de la permutation complète](../../.gitbook/assets/image%20%28135%29.png)

## Tas

### Concepts

Un tas est :

* Un arbre binaire complet.
* Chaque nœud du tas doit être supérieur ou égal (ou inférieur ou égal) à ses nœuds

 enfants. Lorsque chaque nœud est supérieur ou égal, il s'agit d'un tas maximum ; lorsqu'il est inférieur ou égal, il s'agit d'un tas minimum.

**Stockage de tas** : Comme un tas est un arbre binaire complet, le stocker dans un tableau est le plus efficace et économique en termes de mémoire.

### Insertion

Lors de l'insertion d'un élément, il est placé à la fin du tas, ce qui ne respecte pas la définition du tas. Nous devons donc ajuster le tas, ce processus est appelé **enfiler** (heapify).

Après l'insertion, nous devons réaliser une heapification **de bas en haut**. Cela signifie que nous comparons le nœud inséré avec son parent, s'ils ne respectent pas la relation de tas, nous les échangeons, et nous répétons ce processus.

```python
class Heap:
    def __init__(self, capacity):
        self.a = [0] * (capacity + 1) # tableau, stockage des données à partir de l'indice 1
        self.n = capacity # capacité maximale de stockage du tas
        self.count = 0 # nombre de données actuellement stockées dans le tas

    def insert(self, data):
        if self.count >= self.n:
            return # tas plein
        self.count += 1
        self.a[self.count] = data
        i = self.count
        while i // 2 > 0 and self.a[i] > self.a[i // 2]: # heapification de bas en haut
            self.a[i], self.a[i // 2] = self.a[i // 2], self.a[i] # fonction d'échange
            i = i // 2
```

### Suppression de l'élément supérieur du tas

Après la suppression de l'élément supérieur du tas, nous déplaçons le dernier élément à la racine, puis nous procédons à une heapification **de haut en bas**.

```python
def removeMax(self):
    if self.count == 0:
        return -1 # aucune donnée dans le tas
    self.a[1] = self.a[self.count]
    self.count -= 1
    self.heapify(self.a, self.count, 1)

def heapify(self, a, n, i): # heapification de haut en bas
    while True:
        maxPos = i
        if i * 2 <= n and a[i] < a[i * 2]:
            maxPos = i * 2
        if i * 2 + 1 <= n and a[maxPos] < a[i * 2 + 1]:
            maxPos = i * 2 + 1
        if maxPos == i:
            break
        a[i], a[maxPos] = a[maxPos], a[i]
        i = maxPos
```

{% hint style="info" %}
La hauteur d'un arbre binaire complet ne dépasse pas log2n, donc les opérations d'insertion et de suppression de l'élément supérieur du tas ont une complexité temporelle de O(logn).
{% endhint %}

### Tri par tas

Le tri par tas a une complexité temporelle de O(nlogn) et est effectué sur place. Il comporte deux étapes : la construction du tas et le tri.

#### Construction du tas

La construction du tas consiste à transformer un tableau en un tas. Il existe deux approches.

**Approche 1** : Similaire au tri par insertion, divisez le tableau en deux parties, la première moitié forme déjà un tas, puis insérez successivement les données de la deuxième moitié dans le tas. C'est un processus de bas en haut, où les données sont traitées de gauche à droite.

**Approche 2** : Traitement des données de droite à gauche, c'est un processus de haut en bas. Comme les nœuds feuilles n'ont pas de données, nous commençons directement à partir des nœuds internes.

![Construction du tas](../../.gitbook/assets/image%20%28213%29.png)

![Construction du tas - Chemins de parcours](../../.gitbook/assets/image%20%2816%29.png)

```java
private static void buildHeap(int[] a, int n) {
  for (int i = n/2; i >= 1; --i) {
    heapify(a, n, i);
  }
}

private static void heapify(int[] a, int n, int i) {
  while (true) {
    int maxPos = i;
    if (i*2 <= n && a[i] < a[i*2]) maxPos = i*2;
    if (i*2+1 <= n && a[maxPos] < a[i*2+1]) maxPos = i*2+1;
    if (maxPos == i) break;
    swap(a, i, maxPos);
    i = maxPos;
  }
}
```

La complexité temporelle de la construction du tas selon l'approche 2 est O(n). Comme illustré dans le graphique ci-dessous, chaque élément à droite de la ligne médiane est parcouru une fois.

![Complexité temporelle de la construction du tas](../../.gitbook/assets/image%20%2843%29.png)

#### Tri

En prenant le tas maximum comme exemple, nous retirons successivement l'élément racine du tas pour obtenir un tableau trié par ordre croissant.

![Tri par tas](../../.gitbook/assets/image%20%28200%29.png)

```java
public static void sort(int[] a, int n) {
  buildHeap(a, n);
  int k = n;
  while (k > 1) {
    swap(a, 1, k);
    --k;
    heapify(a, k, 1);
  }
}
```

La complexité temporelle de la construction du tas est O(n), tandis que celle du tri est O(nlogn), ce qui donne une complexité totale de O(nlogn) pour le tri par tas. Étant un tri sur place, il n'est pas stable car il échange les éléments, contrairement à un tri stable qui conserve l'ordre relatif des éléments égaux.

{% hint style="warning" %}
Pourquoi le tri rapide est-il plus performant que le tri par tas ?

- Le tri rapide accède aux données de manière continue dans le tableau, tandis que le tri par tas accède aux données de manière discontinue, ce qui rend le tri rapide plus favorable au cache CPU.
- Le nombre d'échanges dans le tri par tas est supérieur à celui dans le tri rapide. Le nombre d'échanges dans le tri rapide est limité par l'inversion, tandis que la construction du tas dans le tri par tas peut perturber l'ordre initial, augmentant ainsi le nombre d'inversions.
{% endhint %}

### Applications du tas

#### File de priorité

Dans une file de priorité, les éléments ne sont pas récupérés dans l'ordre dans lequel ils ont été insérés, mais dans un ordre déterminé par leur priorité.

L'utilisation d'un tas pour implémenter une file de priorité est directe : chaque insertion dans la file de priorité équivaut à une insertion dans le tas. Pour obtenir l'élément de priorité maximale, il suffit de récupérer la racine du tas.

Les files de priorité sont largement utilisées. En voici deux exemples :

**Fusion de petits fichiers triés**

Supposons que vous ayez 100 petits fichiers, chacun de 100 Mo et triés, et que vous souhaitiez les fusionner en un seul gros fichier.

L'idée est similaire à la fusion utilisée dans le tri fusion. À partir des 100 fichiers, vous prenez le premier élément de chaque fichier pour former une file de priorité, qui est également un tas. Ensuite, vous extrayez l'élément de priorité maximale (le plus petit dans ce cas) de la file de priorité et l'écrivez dans le fichier de sortie. Vous prenez ensuite le prochain élément dans le même fichier d'où provient l'élément que vous venez de retirer, l'insérez dans la file de priorité et répétez le processus jusqu'à ce que tous les fichiers soient vides.

**Minimisation du temps de réponse élevé**

Imaginons un minuteur qui gère de nombreuses tâches planifiées, chaque tâche ayant un point de déclenchement dans le temps.

- Implémentation simple : Le minuteur parcourt toutes les tâches à intervalles réguliers. Si une tâche doit être déclenchée, elle est exécutée. Ce processus est inefficace car il doit parcourir toutes les tâches à chaque itération et peut effectuer de nombreux parcours inutiles si la prochaine tâche est loin.
- Implémentation efficace : Utilisez une file de priorité pour stocker les tâches, chaque tâche étant une paire de son délai et de l'action à exécuter. À chaque itération, vérifiez la première tâche dans la file de priorité (celle avec le délai le plus court), attendez jusqu'à ce que le délai soit écoulé, puis exécutez l'action associée. Ensuite, retirez cette tâche de la file de priorité et continuez avec la prochaine tâche.

#### Top K

Trouver les K éléments les plus grands ou les plus petits dans un ensemble de données est un problème courant.

- Données statiques : Maintenez un tas de taille K. Parcourez les données et insérez-les dans le tas. Si le tas dépasse la taille K, retirez l'élément le plus petit. À la fin, le tas contiendra les K éléments les plus

 grands.
- Données dynamiques : La même approche que pour les données statiques peut être utilisée. Chaque fois qu'une donnée est ajoutée, comparez-la à la racine du tas. Si elle est plus grande, remplacez la racine et rééquilibrez le tas.

#### Médiane

Trouver la médiane d'un ensemble de données (l'élément au milieu une fois les données triées) peut également être résolu à l'aide d'un tas.

Maintenez deux tas : un grand tas pour les éléments inférieurs à la médiane et un petit tas pour les éléments supérieurs à la médiane. Les deux tas garantissent que la médiane est toujours entre la racine des deux tas. Lorsqu'un nouvel élément est inséré, comparez-le avec la racine des deux tas pour décider dans lequel le placer. Ensuite, équilibrez les deux tas en déplaçant un élément d'un tas à l'autre si nécessaire.

{% hint style="info" %}
En extrapolant, non seulement la médiane peut être trouvée, mais n'importe quel pourcentage de données peut être calculé. Par exemple, le temps de réponse 99% dans une API.
{% endhint %}

## Union-Find (Union-Find)

L'algorithme Union-Find est une structure arborescente de données utilisée pour résoudre des problèmes de fusion et de recherche sur des ensembles disjoint (Disjoint Sets). Il définit deux opérations :

- **Union** : Fusionner deux sous-ensembles. **Optimisation** : Fusionner le sous-ensemble de plus faible rang dans celui de plus haut rang ; cependant, il suffit généralement d'utiliser l'optimisation de chemin ci-dessous.
- **Find** : Déterminer à quel sous-ensemble appartient un élément, peut être utilisé pour vérifier si deux éléments appartiennent au même sous-ensemble. **Optimisation** : Compression de chemin, lors de la recherche, tous les nœuds sur le chemin sont directement liés à la racine.

Exemples :

- LeetCode 200 : Nombre d'îles.
- LeetCode 547 : Nombre de cercles d'amis.

## Arbre B

L'efficacité de recherche la plus élevée est obtenue avec un arbre binaire, mais il a une hauteur plus élevée et l'indexation ne se fait pas uniquement en mémoire, mais également sur disque, ce qui rend de nombreux systèmes de base de données peu pratiques. Pour réduire le nombre de lectures de disque, N arbres doivent être utilisés, comme environ 1200 pour InnoDB.

Un arbre B \(Balance Tree\) est un arbre de recherche multi-voies équilibré, dont la hauteur est bien inférieure à celle d'un arbre binaire équilibré. Il est largement utilisé dans les systèmes de fichiers et les systèmes de base de données pour implémenter des structures d'index.

Un arbre B de commande M \(M &gt; 2\) peut contenir au maximum M enfants par nœud et présente les caractéristiques suivantes :

- Le nombre d'enfants du nœud racine est compris entre \[2, M\].
- Les nœuds internes ont k - 1 clés et k enfants, avec k dans la plage \[ceil\(M / 2\), M\].
- Les feuilles ont k - 1 clés, avec k dans la plage \[ceil\(M / 2\), M\].
- Les clés des nœuds internes sont stockées dans l'ordre et les k enfants pointent vers les k plages séparées par les clés.
- Toutes les feuilles se trouvent sur la même couche.

## Arbre B+

L'arbre B+ \(B more\) est une amélioration de l'arbre B, largement utilisé dans les systèmes de bases de données comme [MySQL](../../database/mysql/indexing.md#2-mysql-suo-yin). Les améliorations apportées par l'arbre B+ sont les suivantes :

- L'arbre des clés = l'arbre des enfants.
- Les clés des nœuds internes sont également stockées dans les nœuds enfants, la plus grande (ou la plus petite) clé étant dans les nœuds enfants.
- Les nœuds internes ne stockent que les clés, pas les données, qui sont stockées dans les nœuds feuilles.
- Les nœuds feuilles contiennent toutes les clés et forment une liste liée ordonnée entre eux, les nœuds feuilles internes sont également ordonnés.

Avec ces améliorations, les avantages de l'arbre B+ sont les suivants :

- L'efficacité de recherche de l'arbre B+ est plus stable car les données doivent toujours être accédées via les nœuds feuilles.
- Les nœuds internes de l'arbre B+ ne stockent pas de données, de sorte que la même taille de page disque peut stocker plus de clés, ce qui rend l'arbre plus court et plus large, réduisant ainsi le nombre de lectures de disque I/O.
- L'efficacité de la requête par plage est beaucoup plus élevée avec l'arbre B+.

Supposons que nous ayons la table et les données suivantes, l'index InnoDB est illustré dans le diagramme ci-dessous :

```sql
mysql> create table T(
id int primary key, 
k int not null, 
name varchar(16),
index (k))engine=InnoDB;

(100,1) (200,2) (300,3) (500,5) (600,6)
```

![Structure d'index d'InnoDB](../../.gitbook/assets/image%20%28170%29.png)

- **Index primaire** : Aussi appelé index regroupé (clustered index), les nœuds feuilles contiennent des données complètes de lignes.
- **Index secondaire** : Aussi appelé index non regroupé (secondary index), les nœuds feuilles contiennent uniquement les valeurs des clés primaires.

Si une requête est lancée `select * from T where k=5`, elle recherche d'abord dans l'arbre indexé par k, puis récupère l'ID pour rechercher dans l'arbre indexé par ID, ce qui nécessite un **retrour à la source**.

#### Division de la page

Lors de l'insertion de données, par exemple, l'insertion de 700, seule une nouvelle entrée est ajoutée à la fin. Si 400 est inséré, il faut déplacer les données suivantes logiquement. Si la page à insérer

 est pleine, un nouvelle page est allouée, et une partie des données est déplacée vers la nouvelle page.

#### Fusion de la page

Si deux pages adjacentes ont un taux d'occupation très faible en raison de suppressions de données, elles peuvent être fusionnées.

