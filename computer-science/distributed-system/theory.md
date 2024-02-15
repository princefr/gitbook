# Théorie

## Exclusion Mutuelle

À tout moment, seul un programme peut accéder à une ressource, cette méthode d'accès exclusif aux ressources est appelée exclusion mutuelle distribuée, et les ressources partagées sont appelées ressources critiques.

### Algorithme Centralisé

**Idée** : Introduire un coordinateur, chaque fois qu'un accès à une ressource critique est nécessaire, une demande doit être envoyée au coordinateur, qui envoie ensuite des messages d'autorisation en fonction de l'ordre d'arrivée.

**Caractéristiques** : Simplicité et facilité de mise en œuvre, mais l'accessibilité et les performances sont facilement influencées par le coordinateur.

### Algorithme Distribué

**Idée** : Utilisation de la diffusion de groupe et de l'horloge logique, lorsqu'un programme a besoin d'une ressource critique, il envoie une demande à tous les autres nœuds, et les autres nœuds répondent en fonction du moment de la demande.

**Caractéristiques** : Mécanisme d'accès juste avec "premier arrivé, premier servi" et "vote unanime", mais coûts de communication élevés et accessibilité inférieure à celle de l'algorithme centralisé. Convient aux situations où l'utilisation des ressources critiques est peu fréquente et où le système est de petite taille.

### Algorithme de l'Anneau de Jeton

**Idée** : Tous les programmes forment un anneau, le jeton circule dans l'anneau dans le sens des aiguilles d'une montre, un programme recevant le jeton est autorisé à accéder à la ressource critique, puis le transmet au programme suivant. Si un programme n'a pas besoin de la ressource après avoir reçu le jeton, il le transmet immédiatement.

**Caractéristiques** : Grande équité, grande stabilité après l'amélioration des pannes de nœuds uniques. Convient aux systèmes de petite taille où chaque programme utilise fréquemment et brièvement les ressources critiques.

**Amélioration** : Anneau de jeton distribué en deux niveaux, avec une couche externe formant un anneau, chaque nœud de l'anneau étant à son tour un anneau.

## Élection

### Algorithme Bully

Le processus d'élection comprend trois types de messages :

* Élection, initier une élection.
* En vie, réponse à une élection.
* Victoire, annoncer qu'un nœud est devenu le nœud principal.

**Idée** : "Les plus âgés sont les plus forts"

**Prérequis** : Chaque nœud du cluster connaît l'ID de tous les autres nœuds.

Le processus d'élection consiste à ce que chaque nœud vérifie si son ID est le plus grand :

* Si c'est le plus grand, il envoie une Victoire à tous les autres nœuds.
* Sinon :
  * Il envoie une Élection aux nœuds avec des IDs plus grands et attend une réponse :
    * S'il ne reçoit pas de réponse Alive dans un certain délai, il se considère comme le nœud principal et envoie une Victoire.
    * S'il reçoit une Alive des nœuds avec des IDs plus grands, il attend une Victoire.
* Quel que soit le cas, s'il reçoit une Élection d'un nœud avec un ID plus petit, il répond Alive.

**Avantages** : Rapidité d'élection, algorithme simple et facile à implémenter.

**Inconvénients** : Chaque nœud a besoin d'informations globales sur les nœuds. Si un nœud avec un grand ID redémarre fréquemment, cela peut entraîner des élections fréquentes (peut être optimisé).

Exemples : MongoDB, Elasticsearch.

### Raft

Les nœuds ont trois rôles :

* Leader, nœud principal.
* Candidat, prétendant à devenir Leader.
* Follower, ne peut pas initier d'élections.

Idée : "La minorité se soumet à la majorité"

Processus d'élection :

1. Au début, tous les nœuds sont des Followers.
2. Commencez l'élection, tous les nœuds deviennent des Candidats, envoient des demandes d'élection à tous les autres nœuds.
3. Les autres nœuds répondent en fonction de l'ordre d'arrivée des demandes d'élection. Chaque nœud ne peut voter qu'une fois.
4. Le Candidat qui obtient la majorité devient le Leader, les autres deviennent des Followers. Le Leader et les Followers envoient des battements de cœur.
5. Si le mandat du Leader expire ou s'il perd le battement de cœur, une nouvelle élection est lancée.

Avantages : Vitesse d'élection rapide, complexité algorithmique faible, facile à implémenter.

Inconvénients : Chaque nœud doit communiquer avec les autres.

Exemples : etcd, consul.

### ZAB

Exemple : zookeeper.

## Consensus

Le consensus distribué est le processus par lequel plusieurs nœuds peuvent opérer individuellement, mais où tous les nœuds atteignent un consensus sur un état donné.

Le processus d'élection mentionné ci-dessus est une sorte de consensus distribué principalement basé sur le principe du vote à la majorité.

{% hint style="warning" %}
Différence entre la cohérence et le consensus :

* Cohérence : plusieurs nœuds dans un système distribué présentent des données ou un état cohérents à l'extérieur.
* Consensus : processus par lequel plusieurs nœuds dans un système distribué parviennent à un consensus sur un état donné.

Ainsi, la cohérence met l'accent sur le résultat, tandis que le consensus met l'accent sur le processus de parvenir à un accord.
{% endhint %}

### PoW

Proof of Work, preuve de travail. La compétition est basée sur la puissance de calcul, par exemple, tous les nœuds calculent des valeurs satisfaisant une certaine condition, envoient le résultat calculé à d'autres nœuds, et ceux-ci vérifient la valeur. Si la vérification réussit, ils acceptent le droit du nœud. Le nœud avec la plus grande puissance de calcul gagne le droit et diffuse cette information à d'autres nœuds. Comme Bitcoin.

Inconvénients : Longue période pour atteindre un consensus, efficacité faible, consommation de ressources élev

ée.

### PoS

Proof of Stake, preuve d'enjeu. Chaque nœud a des données et une durée de possession des données, qui se transforment en participation.

Inconvénients : Risque de monopole.

### DPoS

Delegated Proof of Stake, preuve d'enjeu déléguée. Les nœuds n'ont pas le droit de fonctionner mais peuvent voter. La participation représente le poids du vote, votant pour d'autres nœuds de confiance.

## Verrouillage

Plusieurs processus accèdent simultanément à une même ressource critique, un seul processus peut accéder à la ressource à la fois.

* Implémentation basée sur les bases de données relationnelles.
* Implémentation basée sur le cache.
* Implémentation basée sur Zookeeper.