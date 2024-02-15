# Couche de transport

Dans l'en-tête IP, il existe un champ de 8 bits pour spécifier le type de protocole, c'est-à-dire si les données dans le paquet IP sont utilisées avec UDP ou TCP.

## UDP

Le **Protocole de Datagramme d'Utilisateur** (UDP, User Datagram Protocol) est un protocole simple de la couche de transport, orienté datagramme.

![UDP](https://gblobscdn.gitbook.com/assets%2F-LtqOV1xW2E02P1V6Hw%2F-Lz_WraW8WVZbFnsrsZ-%2F-Lz_YUgrYdWqibI0OMi2%2Fimage%20%28137%29.png)

Caractéristiques :

- Communication simple : il fait confiance au réseau pour délivrer les données, sans garantie de livraison.
- Sans état : une fois établie, toute connexion peut envoyer ou recevoir des données sans contrainte, même à partir de plusieurs sources.
- Non congestionné : il n'essaie pas de réguler le flux de données en fonction de la congestion du réseau.

Scénarios d'utilisation :

- Faible utilisation de ressources : convient aux réseaux internes où la qualité de la connexion est élevée ou pour les applications insensibles à la perte de données, comme le [DHCP](application-layer.md#dhcp) ou le [PXE](application-layer.md#pxe).
- Applications de diffusion : convient aux applications de diffusion, car il permet de transmettre des données à plusieurs destinataires simultanément. Par exemple, le DHCP utilise la diffusion. De plus, le protocole VXLAN utilise la multidiffusion (classe D des [adresses IP](network-layer.md#les-classes-ip)) et repose également sur UDP.
- Haute vitesse de traitement : convient lorsque la vitesse et la latence sont critiques et qu'une perte de quelques données est tolérable, même en cas de congestion du réseau.

### Exemples d'utilisation de l'UDP

- **QUIC** (Quick UDP Internet Connections) : un protocole de communication proposé par Google, basé sur l'UDP pour réduire la latence des communications réseau et améliorer l'expérience utilisateur.
- **Streaming multimédia** : de nombreux services de streaming en direct utilisent leurs propres protocoles de transmission vidéo basés sur l'UDP pour éviter les retards. Bien que le protocole RTMP soit basé sur TCP, il est sujet à des ralentissements. Ainsi, de nombreuses applications de streaming en direct ont opté pour des protocoles basés sur l'UDP.
- **Jeux en temps réel** : les jeux en ligne ont des exigences strictes en matière de temps réel. Ils utilisent souvent des protocoles UDP personnalisés et fiables avec des stratégies de retransmission pour réduire au minimum l'impact des problèmes réseau sur l'expérience de jeu.
- **IoT (Internet des objets)** : de nombreux terminaux IoT sont des systèmes embarqués avec une faible mémoire. Maintenir des connexions TCP peut être coûteux, et l'IoT nécessite souvent une communication en temps réel. Par exemple, Thread, un protocole de communication IoT développé par le groupe Thread de Google (connu sous le nom de Nest), repose sur UDP.

### TCP

Le **Protocole de Contrôle de Transmission** (TCP, Transmission Control Protocol) est un protocole de communication fiable, orienté connexion et basé sur des flux de données de la couche de transport.

Comparé à l'UDP, il résout cinq problèmes :

- Problème de séquencement
- Problème de perte de paquets
- Maintenance de la connexion
- Contrôle de flux
- Contrôle de congestion

### En-tête TCP

![En-tête TCP](https://gblobscdn.gitbook.com/assets%2F-LtqOV1xW2E02P1V6Hw%2F-Lz_WraW8WVZbFnsrsZ-%2F-Lz_YUgrYdWqibI0OMi2%2Fimage%20%28113%29.png)

- Numéro de séquence : résout les problèmes de désordre des paquets.
- Numéro d'acquittement : résout les problèmes de perte de paquets.
- Drapeaux d'état : SYN est utilisé pour initier une connexion, ACK est utilisé pour répondre, RST est utilisé pour réinitialiser une connexion, FIN est utilisé pour terminer une connexion. L'envoi de paquets avec des drapeaux d'état entraîne des changements d'état des deux côtés de la connexion.
- Taille de la fenêtre : permet le contrôle du flux de données, chaque communication déclare une fenêtre indiquant sa capacité de traitement actuelle.

### Handshake à trois temps

![Handshake à trois temps](https://gblobscdn.gitbook.com/assets%2F-LtqOV1xW2E02P1V6Hw%2F-Lz_WraW8WVZbFnsrsZ-%2F-Lz_YUgrYdWqibI0OMi2%2Fimage%20%2899%29.png)

{% hint style="warning" %}
Pourquoi trois fois ? Le nombre de tentatives de connexion peut être plus élevé, mais peu importe combien de fois cela se produit, cela ne garantit pas la fiabilité. Par conséquent, après trois tentatives de connexion, une fois que les deux parties ont envoyé et reçu une fois, cela suffit.
{% endhint %}

{% hint style="danger" %}
Remarquez les numéros de séquence ; ils ne peuvent pas commencer à partir de 1, car cela peut poser des problèmes. Les numéros de séquence évoluent avec le temps, incrémentant tous les 4 ms.
{% endhint %}

### Quatre fois pour rompre

 une connexion

![Quatre fois pour rompre une connexion](https://gblobscdn.gitbook.com/assets%2F-LtqOV1xW2E02P1V6Hw%2F-Lz_WraW8WVZbFnsrsZ-%2F-Lz_YUgrYdWqibI0OMi2%2Fimage%20%28114%29.png)

### Gestion de la connexion TCP

- **Connexions persistantes** : pour économiser les ressources, HTTP/1.1 parvient à maintenir une connexion persistante en utilisant la même connexion TCP pour envoyer plusieurs requêtes, même si les réponses peuvent être séparées dans le temps.
- **Pool de connexion** : dans les applications modernes, des pools de connexion sont souvent utilisés pour réduire le coût de création et de fermeture des connexions TCP, ce qui est utile pour les applications basées sur les microservices.

### Caractéristiques de TCP

- **Transmission fiable** : garantit que les données sont reçues dans l'ordre et sans erreur.
- **Contrôle de flux** : ajuste le débit de transmission en fonction de la capacité de réception du destinataire pour éviter la congestion.
- **Contrôle de congestion** : régule la vitesse à laquelle les émetteurs envoient des données pour éviter la congestion du réseau.
- **Orienté connexion** : établit une connexion entre les machines source et de destination avant de transférer des données.

### Comparaison entre UDP et TCP

- **UDP** :
  - Plus léger, car il n'a pas besoin de maintenir l'état de la connexion.
  - Convient pour les applications temps réel et de streaming où la latence est critique et la perte de quelques paquets est tolérable.
  - Moins fiable car il ne garantit pas la livraison des données.
  - Meilleur choix lorsque les performances sont primordiales et que la perte de données est acceptable.
- **TCP** :
  - Plus lourd en raison de la surcharge de la gestion de la connexion.
  - Convient pour les applications nécessitant une transmission fiable et un contrôle strict du flux et de la congestion.
  - Fiable car il garantit la livraison des données dans l'ordre sans erreur.
  - Meilleur choix lorsque la fiabilité et l'intégrité des données sont critiques.

### Exemples d'utilisation de TCP

- **HTTP** : le protocole de transfert hypertexte est basé sur TCP, offrant une transmission fiable de contenu Web.
- **FTP** : le protocole de transfert de fichiers utilise TCP pour garantir que les fichiers sont transférés de manière fiable.
- **SSH** : le protocole sécurisé de shell utilise TCP pour fournir un accès shell sécurisé à distance.
- **SMTP** : le protocole de transfert de courrier simple utilise TCP pour transmettre des e-mails de manière fiable entre les serveurs de messagerie.
- **HTTPS** : la version sécurisée de HTTP utilise TCP pour fournir des communications sécurisées sur le Web.
- **Telnet** : un service de connexion distante qui utilise TCP pour permettre aux utilisateurs de se connecter à distance à des hôtes distants.

En résumé, UDP et TCP sont deux protocoles de la couche de transport qui offrent des fonctionnalités différentes pour répondre aux besoins variés des applications réseau. UDP est plus adapté aux applications nécessitant des performances élevées et une latence minimale, tandis que TCP est plus adapté aux applications nécessitant une transmission fiable des données et un contrôle strict du flux et de la congestion.