# File d'attente de messages

## 1. Introduction

### 1.1 Scénarios d'utilisation

Les files d'attente de messages sont des composants importants des systèmes distribués, principalement utilisées pour résoudre quels problèmes ? Ou plutôt, pourquoi utiliser des files d'attente de messages ?

**Désaccouplement :** Si un système A appelle plusieurs systèmes BCD, il est possible qu'un nouveau système E doive être appelé, que d'anciens systèmes D ne doivent plus être appelés, etc. Le système A doit également envisager ce qu'il faut faire si le système C tombe en panne. Est-ce qu'il faut le rappeler ? Cela devient compliqué à maintenir. Si cet appel n'a pas besoin d'être synchrone, il peut être asynchronisé à l'aide d'une file d'attente de messages (MQ) pour désaccoupler.

**Asynchronisme :** En introduisant une file d'attente de messages, les logiques métier non essentielles peuvent être traitées de manière asynchrone, ce qui peut augmenter considérablement le débit du système.

**Lissage des pics :** Cela peut atténuer l'impact des pics de trafic à court terme sur une application, comme lors d'une activité de vente flash.

### 1.2 Inconvénients

L'introduction d'une file d'attente de messages peut avoir les avantages mentionnés ci-dessus, mais cela peut également poser certains problèmes.

**Baisse de disponibilité :** Plus vous introduisez de dépendances externes dans un système, plus il est susceptible de tomber en panne. Si un système A appelle BCD, et qu'une file d'attente de messages est ajoutée, si la file d'attente tombe en panne, tout le système s'effondre.

**Complexité accrue du système :** L'introduction d'une file d'attente de messages nécessite de prendre en compte les problèmes liés à cette file d'attente. Comment garantir qu'aucun message n'est consommé deux fois ? Comment traiter les cas de perte de messages ? Comment garantir l'ordre de transmission des messages ?

**Problèmes de cohérence :** Le système A termine son traitement et envoie un message, puis retourne un résultat réussi à l'utilisateur. Cependant, BCD ne peut pas garantir l'exécution réussie après avoir reçu le message. Si l'un des systèmes échoue, le résultat global est un échec, ce qui n'est pas cohérent avec le résultat obtenu par l'utilisateur.

### 1.3 Comparaison des produits de file d'attente de messages

Les produits de file d'attente de messages couramment utilisés sur le marché sont ActiveMQ, RabbitMQ, RocketMQ, Kafka, etc.

|   Caractéristiques  |   ActiveMQ  |            RabbitMQ            |             RocketMQ            |                        Kafka                       |
| :---: | :---------: | :----------------------------: | :-----------------------------: | :------------------------------------------------: |
| Débit par machine unique |      Des milliers      |               Des milliers               |               Dix mille              |                        Dix mille                        |
| Sujet |             |                                | Les sujets peuvent atteindre des centaines ou des milliers, avec une légère baisse de débit |      <p>Le débit diminue considérablement lorsque le nombre de sujets passe de quelques dizaines à quelques centaines</p><p></p>      |
|  Temps de latence  |     Millisecondes     |               Microsecondes              |               Millisecondes               |                         Millisecondes                        |
|  Disponibilité  |     Élevée, maître-esclave    |              Élevée, maître-esclave              |             Très élevée, distribuée             |                       Très élevée, distribuée                      |
| Fiabilité du message |  Risque de perte de données faible |                                |         Peut être configuré pour une perte de données de 0 après optimisation des paramètres        |                 Peut être configuré pour une perte de données de 0 après optimisation des paramètres                 |
|  Prise en charge des fonctionnalités | Domaine des MQ extrêmement complet | Développé sur Erlang, donc une grande capacité de concurrence, des performances extrêmement bonnes, une faible latence |        Fonctionnalités relativement complètes, toujours distribuées, bonne extensibilité       | Fonctionnalités relativement simples, principalement pour prendre en charge des fonctionnalités MQ simples, largement utilisées dans les domaines du calcul en temps réel et de la collecte de journaux dans le domaine du big data, c'est en fait la norme |

En résumé :

**ActiveMQ :** Autrefois populaire, mais aujourd'hui moins utilisé, n'ayant pas été validé dans des scénarios de débit à grande échelle, une communauté moins active, donc non recommandé.

**RabbitMQ :** Le langage Erlang empêche de nombreux ingénieurs Java de l'étudier et de le maîtriser en profondeur, ce qui le rend presque incontrôlable pour les entreprises. Cependant, il est open source, stable et bien pris en charge, avec une activité élevée.

**RocketMQ :** Garanti par la marque Alibaba, capable de traiter des centaines de milliards de messages par jour, capable de traiter un débit massif, de bonnes performances, facile à étendre. Écrit en Java, donc nous pouvons lire le code source et personnaliser notre propre file d'attente de messages pour notre entreprise.

**Kafka :** Fournit uniquement un nombre limité de fonctionnalités de base, mais offre un débit extrêmement élevé, une latence de l'ordre de la milliseconde, une disponibilité et une fiabilité extrêmement élevées, ainsi qu'une extensibilité distribuée. Dans les domaines du big data, de l'analyse en temps réel, etc., Kafka est la norme de facto, avec une communauté très active, et il est largement utilisé dans le monde entier.

## 2. Garantie de haute disponibilité

### 2.1 Mode maître-esclave

Les produits de file d'attente de messages en mode maître-esclave comprennent ActiveMQ, RabbitMQ, etc. Pour RabbitMQ, il existe trois modes de déploiement : mode mono-instance, mode cluster standard et mode cluster miroir.

**Mode mono-instance :** Niveau démo, utilisé localement pour des tests, peu utilisé en production.

**Mode cluster standard :** Mode cluster par défaut,

 pour les files d'attente, l'entité de message n'existe que sur un nœud A, les autres nœuds BC ont uniquement des métadonnées identiques. Lorsqu'un consommateur tire des messages de B, B les tire d'abord de A, puis les transmet au consommateur. La disponibilité n'est pas garantie, si A tombe en panne, les messages non persistants sont perdus directement, et même avec la persistance, il faut attendre que A redémarre pour fonctionner. Ce mode est principalement utilisé pour augmenter le débit.

**Mode cluster miroir :** Les files d'attente sont configurées comme des files d'attente miroir, existant sur plusieurs nœuds. Il s'agit d'une solution haute disponibilité (HA) de RabbitMQ. Fondamentalement, la différence avec le mode standard est que les messages sont activement synchronisés entre les nœuds miroirs, plutôt que temporairement tirés lorsqu'un consommateur tire des messages.

### 2.2 Mode distribué

Les produits de file d'attente de messages en mode distribué comprennent RocketMQ, Kafka, etc.

Prenons l'exemple de Kafka. Chaque nœud a un processus courtier (Broker). Chaque création de topic sera divisée en plusieurs partitions, les partitions du même topic seront distribuées sur différents courtiers, chaque partition a plusieurs réplicas, et plusieurs réplicas choisiront un leader parmi eux.

Les producteurs et les consommateurs interagissent uniquement avec le leader.

## 3. Consommation de messages en double

RabbitMQ, RocketMQ, Kafka, etc. peuvent tous présenter le problème de la consommation de messages en double. Cependant, ce n'est pas contrôlé par la MQ elle-même, ni ne peut être contrôlé par elle. Cela devrait être géré par l'utilisateur lui-même en garantissant l'**idempotence** de la consommation de messages.

Prenons l'exemple de Kafka pour expliquer pourquoi la consommation de messages en double peut survenir ?
Kafka attribue un décalage (Offset) à chaque message, représentant son numéro de séquence. Après que le consommateur a consommé les données, il soumettra périodiquement l'Offset des messages qu'il a consommés à Kafka. Lors d'un redémarrage anormal du consommateur, s'il n'a pas soumis l'Offset à Kafka avant le redémarrage, il recommencera à consommer à partir de l'Offset où il s'était arrêté la dernière fois. Si le consommateur redémarre sans soumettre l'Offset à Kafka avant le redémarrage, il risque de consommer à nouveau des données.

Assurer l'idempotence de la consommation de messages est une méthode courante :

1. Lors de l'écriture dans la base de données, consultez d'abord la clé principale. Si elle existe, effectuez une mise à jour au lieu d'une insertion.
2. Lors de l'écriture de messages, ajoutez un identifiant unique global à chaque message. Lorsque le consommateur le reçoit, vérifiez d'abord dans Redis s'il a déjà été consommé.

## 4. Perte de messages

Le principe fondamental de la MQ est que les données ne peuvent ni être ajoutées ni perdues. Ne pas pouvoir ajouter de données signifie la consommation en double mentionnée ci-dessus, ne pas pouvoir perdre de données signifie que les messages ne peuvent pas être perdus.

La perte de messages peut se produire à trois étapes : producteur, MQ, consommateur. Explorons comment RabbitMQ et Kafka résolvent le problème de la perte de messages.

### 4.1 RabbitMQ

#### 4.1.1 Perte de données du producteur

Lorsque le producteur envoie des données à RabbitMQ, il peut perdre des données en raison de problèmes réseau en cours de route.

**Mode transactionnel :** Avant d'envoyer un message, ouvrez une transaction `channel.txSelect`, puis envoyez le message. Si RabbitMQ ne reçoit pas le message, une exception sera levée. À ce stade, le message peut être annulé avec `channel.txRollback`, puis renvoyé. Si le message est reçu, la transaction peut être validée avec `channel.txCommit`.

**Mode de confirmation :** Le côté producteur active le mode de confirmation. Chaque fois qu'un message est écrit, un ID lui est attribué. Après le traitement par RabbitMQ, il envoie un rappel (`ack, nack`) au producteur. Le producteur peut alors maintenir l'état de chaque message dans la mémoire. Si aucun rappel n'est reçu pendant une longue période, il peut être renvoyé.

Le mode transactionnel est synchrone, ce qui réduit le débit de la MQ. La confirmation est asynchrone. Il est donc généralement recommandé d'utiliser le mode de confirmation.

#### 4.1.2 Perte de données de la MQ

Activez la fonctionnalité de persistance. Lors de la création d'une file d'attente, configurez-la pour être persistante, et lors de l'envoi d'un message, configurez également le message pour qu'il soit persistant (`deliveryMode = 2`). Les deux doivent être configurés pour assurer la persistance.

Si la persistance n'est pas terminée et que RabbitMQ tombe en panne, les messages seront également perdus, mais cette probabilité est très faible. La confirmation peut être combinée avec le mode de persistance pour ne renvoyer le rappel au producteur qu'après la persistance complète.

#### 4.1.3 Perte de données du consommateur

Si un consommateur reçoit un message et tombe en panne avant de le traiter, mais que RabbitMQ pense que vous avez déjà terminé le traitement, cela entraînera une perte de données.

Désactivez l'ACK automatique de RabbitMQ, ce qui peut être fait via l'API. Envoyez l'ACK à RabbitMQ uniquement après avoir traité le message.

### 4.2 Kafka

#### 4.2.1 Perte de données du producteur

`acks=all`, exige que chaque donnée soit écrite sur tous les réplicas avant d'être considérée comme écrite avec succès.

`retries = Integer.MAX`, ce qui signifie que s'il y a un échec d'écriture, il doit être réessayé indéfiniment.

#### 4.2.2 Perte de données de la MQ

Si un courtier tombe en panne et que le leader dessus tombe

 également en panne, il est nécessaire de réélire un leader, mais à ce moment-là, certains followers n'ont pas encore synchronisé toutes les données, ce qui peut entraîner une perte de données.

Configurez quatre paramètres :

1. `Paramètre topic : replication.factor > 1`, chaque partition du topic doit avoir au moins 2 réplicas.
2. `Paramètre côté serveur Kafka : min.insync.replicas > 1`, exige qu'un leader perçoive au moins un follower pour maintenir la connexion, de sorte qu'il puisse y avoir des followers à élire après la panne du leader.
3. `Paramètre côté producteur : acks = all`.
4. `Paramètre côté producteur : retries = Integer.MAX`.

#### 4.2.3 Perte de données du consommateur

Similaire à RabbitMQ, le consommateur soumettra automatiquement l'Offset, il suffit de désactiver la soumission automatique et de soumettre l'Offset manuellement après le traitement du message.

## 5. Garantie de l'ordre des messages

### 5.1 RabbitMQ

Si un seul consommateur lit de multiples messages d'une seule file d'attente, l'ordre des messages sera perturbé. RabbitMQ ne garantit pas l'ordre de consommation, car cela aurait un coût prohibitif. Par conséquent, l'ordre doit être garanti au niveau de l'application.

**Méthode 1 :** Diviser en plusieurs files d'attente, chaque file d'attente correspondant à un consommateur.

**Méthode 2 :** Une file d'attente correspond à un consommateur, puis le consommateur interne trie les messages avant de les distribuer aux travailleurs.

### 5.2 Kafka

Kafka a une caractéristique selon laquelle une partition ne peut être consommée que par un consommateur fixe.

**Ordonné globalement :** Par exemple, la transmission du journal binaire MySQL utilise généralement un producteur, une partition, un consommateur. Bien sûr, différentes tables peuvent utiliser différents sujets ou partitions.
