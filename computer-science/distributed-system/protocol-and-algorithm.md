# Protocole et Algorithme

## Aperçu

Pour un algorithme distribué, nous analysons généralement depuis les perspectives de la tolérance aux fautes byzantines, de la cohérence, des performances et de la disponibilité.

### Tolérance aux Fautes Byzantines

Décrivant un scénario non fiable où non seulement des pannes se produisent mais aussi des comportements malveillants.

La plupart des environnements (intranet d'entreprise) sont fiables, il suffit que le système ait une tolérance aux pannes.

Dans un environnement non fiable, les algorithmes de tolérance aux fautes byzantines courants incluent POW, PBFT, etc.

### Cohérence

* **Cohérence Forte (Strong consistency) :** Une fois qu'une opération d'écriture est terminée, toutes les opérations de lecture ultérieures peuvent lire la valeur mise à jour. Cela inclut :
  * La cohérence linéaire (Linearizability consistency), la cohérence atomique : Le "C" dans CAP fait référence à cela, comme les opérations d'écriture de ZK, les opérations de lecture/écriture d'etcd.
  * Cohérence séquentielle (Sequential consistency) : Comme le système global de ZK (lecture+écriture).
* **Cohérence Faible (Weak consistency) :** Une fois qu'une opération d'écriture est terminée, il n'y a aucune garantie que les opérations de lecture ultérieures obtiendront la valeur la plus récente. Cela inclut :
  * Cohérence causale (Causal consistency)
  * Cohérence éventuelle (Eventual consistency) : Si un objet n'a plus d'opérations d'écriture, toutes les opérations de lecture ultérieures obtiendront finalement la valeur la plus récente.

La définition de la cohérence a des significations différentes dans différentes théories, ce qui peut être facilement confondu et nécessite une clarification. Vous pouvez consulter :
* [http://kaiyuan.me/2018/04/21/consistency-concept/](http://kaiyuan.me/2018/04/21/consistency-concept/)
* [https://segmentfault.com/a/1190000022248118](https://segmentfault.com/a/1190000022248118)

### Disponibilité

Gossip permet à un seul nœud de fournir un service ; Paxos, ZAB, Raft, Quorum NWR, PBFT, POW peuvent tolérer un certain nombre de pannes de nœuds ; 2PC, TCC ne peuvent fonctionner que lorsque tous les nœuds sont en bonne santé.

### Performance

Gossip est un système AP, offrant la meilleure performance ; Paxos, ZAB, Raft fonctionnent tous sur un modèle de leader, où les performances d'écriture sont limitées par le leader et les performances de lecture dépendent de la mise en œuvre de la cohérence ; 2PC, TCC nécessitent une réservation et un verrouillage des ressources, offrant des performances plus faibles.

## Problème des Généraux Byzantins

Il décrit le scénario de panne distribuée le plus complexe, où non seulement des pannes se produisent mais aussi des comportements malveillants. Les algorithmes couramment utilisés sont l'algorithme PBFT et l'algorithme PoW.

Un exemple est donné par le problème des 2 fidèles et 1 traître.

### Basé sur les Messages Oraux

Mentionné dans le document de Lamport [The Byzantine Generals Problem](https://www.microsoft.com/en-us/research/publication/byzantine-generals-problem/).

Prérequis de l'algorithme : le nombre de traîtres m est connu. Et cela nécessite m+1 tours de récursion.

Conclusion de l'algorithme : si le nombre de traîtres est m, alors le nombre total de généraux ne peut pas être inférieur à 3m + 1.

Donc, pour le problème des 2 fidèles et 1 traître, il est nécessaire d'ajouter un général fidèle supplémentaire pour résoudre le problème.

### Basé sur les Signatures des Messages

Caractéristiques des messages :

* Les messages ne peuvent pas être falsifiés, et toute modification des messages peut être détectée.
* Tout le monde peut vérifier l'authenticité des messages.

Sur la base de ces caractéristiques, le document de Lamport indique que tout message falsifié peut être détecté, et que quel que soit le nombre de fidèles ou de traîtres, les généraux fidèles peuvent toujours parvenir à un consensus sur les messages de commandement, c'est-à-dire que n généraux peuvent tolérer n-2 traîtres. Cela nécessite également m+1 tours de négociation. Ce problème résout comment les généraux fidèles peuvent parvenir à un consensus, sans se soucier de ce que ce consensus représente, ce qui est plutôt théorique.

{% hint style="info" %}
La signature des messages est généralement implémentée par cryptographie asymétrique. Par exemple, lorsque A envoie un message à B, A a une clé privée et B a une clé publique. A calcule la valeur de hachage (MD5) du message, puis l'encrypte avec sa clé privée, et envoie à la fois le message et le hachage chiffré. B déchiffre le hachage avec la clé publique, puis calcule également le hachage du message, comparant les deux hachages pour vérifier l'intégrité du message.
{% endhint %}

## Théorie CAP

Il abstrait les caractéristiques des systèmes distribués, à savoir la cohérence, la disponibilité et la tolérance aux partitions, et résume les conflits entre ces caractéristiques, nous permettant de peser entre la cohérence des données (ACID) et la disponibilité du service (BASE).

### Cohérence (Consistency)

À chaque opération de lecture du client, quel que soit le nœud auquel il accède, il lit soit la même version des données la plus récente, soit échoue.

On peut considérer cela comme un engagement des systèmes distribués envers leurs clients : peu importe le nœud que vous accédez chez moi, je vous renvoie des données qui sont absolument cohérentes et les plus récemment écrites, sinon votre lecture échoue.

### Disponibilité (Availability)

Le client peut obten

ir une réponse de n'importe quel nœud non en panne, mais il n'est pas garanti qu'il s'agisse de la même version des données les plus récentes.

{% hint style="success" %}
Je pense que cela garantit la disponibilité de la lecture et de l'écriture en même temps. Si seulement l'une des deux est garantie, alors on peut garantir à la fois C et A.
{% endhint %}

### Tolérance aux Partitions (Partition Tolerance)

Lorsqu'il y a une perte de messages ou des délais élevés entre les nœuds, le système continue de fonctionner.

La tolérance aux partitions est une exigence essentielle des systèmes distribués.

### Analyse

L'article "Brewer's conjecture and the feasibility of consistent, available, partition-tolerant web services" prouve que ces trois indicateurs ne peuvent pas être atteints simultanément, seulement deux peuvent être choisis. Notez que l'article définit la cohérence comme la cohérence atomique.

* Lorsque vous choisissez un système CP, en cas de défaillance du réseau entre les nœuds, pour ne pas compromettre la cohérence, le système peut ne pas répondre aux requêtes de lecture du client.
* Lorsque vous choisissez un système AP, en cas de défaillance du réseau entre les nœuds, le système continuera de répondre aux requêtes des clients, mais certains nœuds peuvent ne pas renvoyer les données les plus récentes.
* Le modèle CA abandonne la distribution, comme une version autonome de MySQL.

{% hint style="success" %}
Pour les systèmes CP, je pense qu'il est possible de garantir à la fois la C et la A lorsqu'il n'y a pas de partition. Autrement dit, lorsque P se produit, l'opération d'écriture échoue, de sorte que tout nœud recevant une demande de lecture peut répondre (garantir A) et renvoyer les données écrites avec succès les plus récentes (garantir C).
{% endhint %}

{% hint style="warning" %}
Beaucoup de gens ont **mal interprété** CAP : dans toutes les circonstances, les systèmes distribués ne peuvent choisir qu'entre C et A.

Lorsqu'il n'y a pas de partition réseau (état dans lequel se trouve le système la plupart du temps), C et A peuvent être simultanément garantis.
{% endhint %}

Pour la version cluster d'InfluxDB, le cluster Meta adopte la conception du système CP, tandis que le cluster Data adopte la conception du système AP.

{% hint style="info" %}
* Vous pouvez considérer ACID comme l'exigence de cohérence dans les systèmes CP de la théorie CAP.
* Vous pouvez considérer BASE comme une extension de la théorie CAP pour les systèmes AP.
{% endhint %}

## ACID

La théorie ACID est une abstraction et une synthèse des caractéristiques des transactions. On peut comprendre que, qu'il s'agisse de systèmes autonomes ou distribués, dès lors que les propriétés ACID des opérations sont implémentées, alors ce système réalise des transactions.

* Atomicité : Les opérations de traitement des données sont des unités de base et ne peuvent pas être divisées. Une transaction a plusieurs opérations, soit elles sont toutes exécutées avec succès, soit aucune ne l'est (ce qui nécessite une opération de rollback).
* Cohérence : Après des opérations de transaction, la base de données passe d'un état cohérent à un autre. La cohérence est généralement définie par l'**entreprise**. Le "C" ici est une cohérence forte, mais sa mise en œuvre dans les systèmes distribués est difficile, ce qui a conduit à l'émergence de la théorie BASE.
* Isolation : Les transactions ne sont pas affectées par d'autres transactions.
* Durabilité : Une fois qu'une transaction est validée, les modifications des données sont durables.

Les principes, la mise en œuvre, etc. des transactions sur une seule machine sont décrits dans le chapitre MySQL. La mise en œuvre et les algorithmes des transactions distribuées sont décrits dans le chapitre des transactions distribuées.

## BASE

La disponibilité d'un système CP est le produit de la disponibilité de tous les nœuds du système. Plus il y a de nœuds, moins le système est disponible, donc il vaut mieux choisir un système AP.

La théorie BASE est le résultat de l'équilibrage entre la cohérence des données (ACID) et la disponibilité du service (BASE) dans la théorie CAP. Elle est basée sur CAP et son cœur est la **disponibilité de base** (Basically Available) et la **cohérence finale** (Eventually Consistent).

{% hint style="info" %}
L'état mou (Soft State) décrit un état de transition des données lors de la mise en œuvre de la disponibilité du service, où des répliques de données peuvent être momentanément incohérentes entre différents nœuds.
{% endhint %}

### Disponibilité de Base

En cas de pannes imprévisibles dans un système distribué, une partie de la fonctionnalité peut être perdue, mais la disponibilité de la fonctionnalité principale est garantie.

La disponibilité de base est essentiellement un compromis, et les méthodes courantes pour la réaliser incluent : **le lissage du trafic**, **la réponse retardée**, **la dégradation de l'expérience utilisateur**, **la protection contre la surcharge**.

### Cohérence Finale

Toutes les répliques de données du système atteignent finalement un état cohérent après une période de synchronisation, c'est-à-dire qu'il y a un léger délai dans la cohérence des données.

La plupart des systèmes Internet adoptent une cohérence finale, seuls des cas où la cohérence finale est insuffisante conduiront à l'utilisation de la cohérence forte, comme lorsque des métadonnées sensibles du système sont gérées avec une cohérence forte, ou lorsque des données de paiement ou financières sont gérées avec des transactions.

Vous pouvez considérer la cohérence forte comme une cohérence finale sans délai.

Les critères de décision pour la cohérence finale des données ont généralement deux méthodes :

* Les données les plus récemment écrites.
* Les données écrites pour la première fois.

Les méthodes courantes pour implémenter la cohérence finale sont :

* Réparation à la lecture : comme la réparation à la lecture de Cassandra. Il est nécessaire d'optimiser autant que possible

 l'algorithme de comparaison de cohérence des données.
* Réparation à l'écriture : comme le transfert d'indications. Il n'est pas nécessaire de comparer les données, ce qui améliore les performances, donc cette méthode est préférée.
* Réparation asynchrone : vérification périodique des comptes. Il est nécessaire d'optimiser autant que possible l'algorithme de comparaison de cohérence des données.

{% hint style="info" %}
Lors de la mise en œuvre de la cohérence finale, il est recommandé de définir un niveau de cohérence d'écriture, afin de permettre aux utilisateurs de choisir, comme All, Quorum, One, Any.
{% endhint %}

## Paxos Basique

Trois rôles :

* Proposant (Proposer) : Le nœud du cluster qui reçoit les demandes des clients.
* Acquéreur (Acceptor) : Il vote pour chaque proposition de valeur et stocke les valeurs acceptées.
* Apprenant (Learner) : Informé du résultat du vote, il accepte les valeurs qui ont atteint un consensus et les stocke.

> Généralement, les nœuds du cluster assument plusieurs rôles, comme proposant & acquéreur. Par exemple, dans un cluster de 3 nœuds, le nœud 3 accepte les demandes des clients, agissant en tant que proposant, et ce nœud ainsi que les deux autres nœuds agissent en tant qu'acquéreur.