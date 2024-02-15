# Recherche de chaînes

Recherche de la chaîne B dans la chaîne A, où A est appelée la **chaîne principale** et B est appelée le **patron**.

## BF

Brute Force, algorithme de recherche brute ou algorithme de recherche naïve.

**Idée** : La longueur de la chaîne principale est n, la longueur du patron est m. Dans la chaîne principale, à partir de la position initiale 0, 1, 2 ... n-m et avec une longueur de m, on regarde s'il y a une correspondance avec le patron.

La complexité temporelle la plus défavorable est O\(n\*m\), dans le pire des cas, il faut comparer m caractères à chaque fois, ce qui nécessite m - n + 1 comparaisons.

{% hint style="info" %}
Bien que la complexité temporelle de l'algorithme BF soit élevée, il est souvent utilisé dans le développement réel pour les raisons suivantes :

* Dans la plupart des cas, ni la chaîne principale ni le patron ne sont très longs.
* À chaque comparaison du patron avec une sous-chaîne de la chaîne principale, s'il y a une non-correspondance, on peut arrêter prématurément, ce qui évite généralement d'atteindre la complexité temporelle maximale.
* L'idée est simple, l'implémentation est simple, ce qui réduit les risques d'erreurs.
{% endhint %}

## RK

L'algorithme Rabin-Karp, nommé d'après ses deux inventeurs.

Dans l'algorithme BF, chaque fois que le patron est comparé avec une sous-chaîne de la chaîne principale, chaque caractère du patron doit être comparé. L'idée centrale de RK est de calculer les valeurs de hachage de n - m + 1 sous-chaînes, puis de comparer les valeurs de hachage avec celle du patron, ce qui accélère considérablement la comparaison.

Cependant, le calcul des valeurs de hachage pour toutes les sous-chaînes nécessite toujours de parcourir chaque caractère de chaque sous-chaîne, ce qui nécessite la conception d'un algorithme de hachage pour optimiser cela.

L'idée principale est que **le calcul des valeurs de hachage peut utiliser le résultat de calcul de la sous-chaîne précédente**. Par exemple : supposons que l'ensemble de caractères à rechercher ne comporte que K caractères, on peut utiliser un nombre en base K pour représenter un caractère, converti en nombre en base 10 comme valeur de hachage.

La complexité temporelle est O\(n\).

Si l'on considère les collisions de hachage, après l'égalité des valeurs de hachage, il est encore nécessaire de comparer les chaînes originales.

## BM

L'algorithme Boyer-Moore comprend deux parties, la règle du mauvais caractère (bad character rule) et la règle du bon suffixe (good suffix shift).

Le processus de correspondance du patron et de la chaîne principale peut être vu comme le patron se déplaçant continuellement vers l'arrière dans la chaîne principale. Lorsqu'une non-correspondance est rencontrée, les algorithmes BF et RK déplacent le patron d'une position vers l'arrière ; l'essence de l'algorithme BM est de chercher des règles pour permettre au patron de se déplacer vers l'arrière le plus possible.

### Règle du mauvais caractère

Tout d'abord, on fait correspondre en commençant par les index du patron en ordre décroissant. Lorsqu'un caractère (le **mauvais caractère**) ne peut pas correspondre, la position du mauvais caractère dans le patron est notée si, puis ce mauvais caractère est recherché **depuis l'arrière** dans le patron pour trouver l'index xi. Si xi n'est pas trouvé, xi = -1, alors le patron est déplacé si - xi positions vers l'arrière.

Dans l'exemple ci-dessous, lors de la correspondance en partant de la fin, c est le mauvais caractère, si = 2, xi = -1, donc le patron est déplacé de 3 positions vers l'arrière. Après le déplacement, la correspondance se poursuit ; a est le mauvais caractère, si = 2, xi = 0, donc le patron est déplacé de 2 positions vers l'arrière.

```text
a b c a c a b d c
a b d
      a b d
          a b d
```

**Meilleure complexité temporelle** : O\(n/m\). Chaîne principale : aaabaaabaaab, patron : aaaa.

**Problème** : Chaîne principale : aaaaaaaaaaa, patron : baaa, selon l'algorithme précédent, il y aurait encore un déplacement vers l'arrière.

### Règle du bon suffixe

**Sous-chaîne suffixe** : correspondance en alignant le dernier caractère, par exemple, abc a pour sous-chaînes suffixes c, bc.  
**Sous-chaîne préfixe** : correspondance en alignant le premier caractère, par exemple, abs a pour sous-chaînes préfixes a, ab.

Lorsque le patron est comparé à la chaîne principale de l'arrière vers l'avant, en cas de non-correspondance, une partie de la correspondance est déjà effectuée à l'avance, cette partie de la correspondance est appelée **bon suffixe**. Par exemple, avec le mauvais caractère a, le bon suffixe est ca.

```text
a b c a c a b d c
    d b c a
```

**Principe** : La partie du bon suffixe du patron déjà appariée est notée {u}, puis le bon suffixe continue à être recherché dans le patron pour trouver un autre {u\*} correspondant :

* Si {u\*} peut être trouvé, alors le patron est glissé jusqu'à ce que {u\*} soit aligné avec le bon suffixe dans la chaîne principale.
* Si {u\*} ne peut pas être trouvé, on vérifie si **une sous-chaîne suffixe du bon suffixe** correspond à **une sous-chaîne préfixe du patron**, et on trouve **la plus longue {v}**, puis le patron est glissé jusqu'à ce que {v} soit aligné avec la partie correspondante {v

} dans la chaîne principale.

```text
// {u} = de, cas 1 où {u*} peut être trouvé
a b c d e f g
d e a d e
      d e a d e

// {u} = de, cas 2 où {u*} ne peut pas être trouvé, {v} = d
a b c d e f g
e a a d e
        e a a d e
```

{% hint style="info" %}
Calculer séparément les longueurs de déplacement des règles du bon suffixe et du mauvais caractère, puis choisir le déplacement le plus long.
{% endhint %}

## KMP

L'algorithme KMP est essentiellement le même que l'algorithme KM. Pendant le processus de correspondance du patron et de la chaîne principale, s'il y a une non-correspondance, on cherche des règles pour permettre au patron de se déplacer le plus en arrière possible.

## Arbre Trie

L'arbre Trie, également appelé arbre de préfixes. Une structure de données spécialement conçue pour la recherche de chaînes, résolvant le problème de la recherche rapide d'une chaîne dans un ensemble de chaînes.

**Essence** : En utilisant les préfixes communs entre les chaînes, les préfixes répétés sont fusionnés.

Comme illustré ci-dessous, le nœud racine ne contient aucune information, chaque nœud représente un caractère dans une chaîne, le chemin de la racine au nœud rouge représente une chaîne. Note : **le nœud rouge n'est pas nécessairement une feuille**.

![](../../.gitbook/assets/image%20%28214%29.png)

```java
// Supposons que la chaîne ne contienne que 26 lettres minuscules de l'alphabet anglais. Cette méthode de stockage consomme beaucoup de mémoire.
// Les nœuds peuvent être remplacés par d'autres structures de données pour économiser de la mémoire, comme les listes chaînées, les tables de hachage, les arbres rouge-noir, etc.
class TrieNode {
  char data;
  TrieNode children[26];
  public boolean isEndingChar = false;
}
```

**Complexité temporelle** : La construction de l'arbre Trie est O\(n\), où n représente la somme des longueurs de toutes les chaînes, la construction ne nécessite qu'une seule fois ; la recherche est O\(k\), où k est la longueur de la chaîne à rechercher.

**Conditions d'application** :

* L'ensemble de caractères ne doit pas être trop grand, sinon beaucoup de mémoire sera consommée ; si les nœuds sont remplacés par d'autres structures de données, cela réduira l'efficacité.
* Les préfixes des chaînes sont souvent répétés.

**Scénarios d'application** :

* Auto-complétion
* Suggestions de mots-clés de recherche

**Comparaison avec les tables de hachage, les arbres rouge-noir** : La correspondance précise convient aux structures de données telles que les tables de hachage et les arbres rouge-noir ; la correspondance de préfixe convient à l'arbre Trie.

### Exemple

* [LeetCode 208 : Implémentation de l'arbre Trie.](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/tree/LT208.java)

## Automate AC

L'algorithme Aho–Corasick est une amélioration de l'arbre Trie.