# Couche liaison de données

La **couche liaison de données** est la deuxième couche du modèle OSI. Elle peut être subdivisée en deux sous-couches : la sous-couche de contrôle d'accès au support (MAC) et la sous-couche de contrôle de liaison logique (LLC).

La sous-couche de contrôle d'accès au support (**MAC**, pour *Medium Access Control*) gère exclusivement les problèmes de contention et de conflit d'accès au support. C'est la partie inférieure de la couche liaison de données.

Lorsqu'on parle de hub dans la couche physique, il copie le signal et l'envoie à toutes les interfaces, donc il faut résoudre les trois problèmes suivants :

* Qui devrait recevoir ce paquet de données ?
* Comment résoudre les conflits lorsque plusieurs machines envoient simultanément des données ? C'est le problème de l'accès multiple.
* Comment résoudre les erreurs d'envoi ?

Les méthodes pour résoudre le problème de l'accès multiple sont :

* La division du canal.
* Les protocoles de rotation.
* Les protocoles d'accès aléatoire, comme Ethernet.

![Format de la couche liaison de données](../../.gitbook/assets/image%20%2866%29.png)

Pour résoudre le premier problème, il faut se fier à l'adresse MAC de destination du paquet réseau.

Pour résoudre le troisième problème, on utilise le CRC (Cyclic Redundancy Check), une technique de contrôle d'erreur qui utilise une somme de contrôle pour détecter les erreurs de transmission.

## ARP

Le **protocole de résolution d'adresses** (**ARP**, pour *Address Resolution Protocol*) est un protocole de transmission de réseau qui permet de rechercher l'adresse de couche liaison de données (MAC) à partir de l'adresse de couche réseau (IP), c'est-à-dire de trouver l'adresse MAC à partir de l'adresse IP.

En envoyant une diffusion, toute machine dont l'IP correspond répondra avec son adresse MAC.

Pour éviter de devoir effectuer une requête ARP à chaque fois, les machines conservent également un cache ARP localement. Bien sûr, les machines peuvent se connecter et se déconnecter, et les adresses IP peuvent changer, donc le cache d'adresses MAC ARP expire après un certain temps.

## Commutateur

C'est un appareil de la couche 2.

Lorsqu'un ordinateur MAC1 envoie un paquet à un autre ordinateur MAC2, lorsque ce paquet arrive au commutateur, celui-ci ne sait pas au départ où se trouve l'ordinateur MAC2. Il enverra donc le paquet à tous les ports sauf celui par lequel il est arrivé. Cependant, à ce moment-là, le commutateur se souviendra que MAC1 provient d'un port spécifique. Plus tard, si un paquet a une adresse de destination MAC1, il sera envoyé directement à ce port.

Comme une porte de péage, après un certain temps, le réseau aura une structure complète, et à ce moment-là, il n'y aura pratiquement plus de diffusion. Tout peut être transmis avec précision. Bien sûr, chaque machine aura une adresse IP qui peut changer, et donc les résultats de l'apprentissage sur le commutateur, que nous appelons une **table de commutation**, ont une durée de vie.

## STP

Lorsque le nombre de machines augmente, un seul commutateur ne suffit pas, donc de plus en plus de commutateurs apparaîtront, et lorsque le réseau de connexion deviendra plus complexe, des problèmes de **boucles de commutation** pourront survenir.

Le **protocole d'arbre couvrant** (**STP**, pour *Spanning Tree Protocol*) est un protocole de communication de la couche 2 (couche liaison de données) du modèle OSI, dont l'application de base est d'empêcher les boucles de commutation redondantes, assurant ainsi qu'aucun réseau Ethernet n'a de boucles logiques et permettant d'éviter les tempêtes de diffusion.

**Principe de fonctionnement** : Dans chaque commutateur, si deux ou plusieurs chemins vers le commutateur racine sont disponibles, le protocole STP coupe l'un des chemins en fonction de l'algorithme, ne laissant qu'un seul chemin, garantissant ainsi qu'il n'y a qu'un seul chemin actif entre chaque paire de commutateurs.

**Processus de fonctionnement** : Tout d'abord, l'élection du commutateur racine est effectuée, basée sur la priorité du commutateur (bridge priority) et l'ID de pont généré en combinaison avec l'adresse MAC. Le commutateur avec l'ID de pont le plus bas devient le commutateur racine dans le réseau. Sur cette base, les distances de chaque nœud au commutateur racine sont calculées, et les coûts des chemins redondants sont obtenus à partir de ces chemins, le chemin le plus court étant sélectionné comme chemin de communication (le port correspondant change d'état pour devenir *forwarding*), et les autres deviennent des chemins de secours (le port correspondant change d'état pour devenir *blocking*). Les tâches de communication du processus de génération STP sont effectuées par des BPDUs (Bridge Protocol Data Units), qui se divisent en BPDUs de configuration contenant des informations de configuration (leur taille ne dépasse pas 35 octets) et des BPDUs de notification contenant des informations de changement de topologie (leur taille ne dépasse pas 4 octets).

* **Root Bridge**, ou **commut

ateur racine**.
* **Designated Bridges**, parfois traduit par **commutateurs désignés**.
* **Bridge Protocol Data Units (BPDU)**, ou **unités de données de protocole de pont (BPDU)**.
* **Priority Vector**, ou **vecteur de priorité**.

## VLAN

Lorsque le nombre de commutateurs augmente, des problèmes de diffusion et de sécurité peuvent survenir.

Le **réseau local virtuel** (**VLAN**, pour *Virtual Local Area Network*) est une technologie de gestion de réseau construite sur la technologie de commutation LAN (LAN Switch), qui permet aux administrateurs réseau de contrôler efficacement le routage des paquets entrants et sortants du réseau local vers les ports d'entrée et de sortie appropriés, permettant de **regrouper logiquement les appareils dans différents réseaux locaux physiques**, ce qui réduit le nombre de paquets inutiles circulant dans le réseau local et **améliore la sécurité de l'information** du réseau local.

![VLAN](../../.gitbook/assets/image%20%28143%29.png)

Un TAG est ajouté au-dessus de la couche 2 existante, contenant un ID VLAN sur 12 bits. Si un commutateur prend en charge les VLAN, il retirera l'en-tête de la couche 2 et identifiera l'ID VLAN. Seuls les paquets appartenant au même VLAN seront transmis entre eux, et les paquets de VLAN différents ne seront pas visibles. Cela permet de résoudre les problèmes de diffusion et de sécurité.

Vous pouvez configurer le VLAN auquel chaque port du commutateur appartient. Chaque port VLAN peut être réaffecté.

Les connexions entre les commutateurs se font via les ports **Trunk**, qui peuvent transmettre des paquets de n'importe quel VLAN.

## Conclusion

* La couche MAC est utilisée pour résoudre les problèmes de congestion de l'accès multiple.
* L'ARP permet de trouver l'adresse MAC de destination en criant, et après avoir crié pendant un certain temps, cela est enregistré dans un cache, appelé cache ARP.
* Les commutateurs ont la capacité d'apprentissage des adresses MAC, une fois qu'ils ont appris, ils savent où sont les choses, et il n'y a pas besoin de diffusion.
* Lorsque le nombre de commutateurs augmente, des problèmes de boucles de connexion peuvent survenir, qui sont résolus en utilisant le protocole STP pour transformer le graphique en un arbre sans boucle.
* Les VLAN peuvent former des réseaux locaux virtuels pour résoudre les problèmes de diffusion et de sécurité.