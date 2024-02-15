# Protocole Réseau

Les cours fondamentaux de l'informatique à l'université comprennent "Organisation et Architecture des Ordinateurs", "Structures de Données et Algorithmes", "Systèmes d'Exploitation", "Réseaux Informatiques" et "Compilation".

Avec l'émergence continue de nouvelles technologies, la connaissance des protocoles réseau reste précieuse même après l'âge de 45 ans.

Les trois éléments essentiels d'un protocole sont :

- Syntaxe : Un contenu doit respecter des règles et un format spécifiques, par exemple, les parenthèses doivent être appariées.
- Sémantique : Un contenu doit représenter une signification particulière, par exemple, la soustraction de nombres a une signification.
- Séquence.

Ce n'est qu'avec des protocoles réseau que de nombreux appareils peuvent coopérer et accomplir une tâche commune.

À l'origine, les concepteurs de TCP/IP ont proposé une architecture en couches comprenant quatre couches. TCP/IP a été inventé dans les années 1970, à une époque où de nombreux protocoles réseau existaient encore, rendant le paysage des réseaux plutôt chaotique. À ce moment-là, l'Organisation Internationale de Normalisation (ISO) a envisagé une normalisation globale et a donc conçu OSI, le modèle de référence pour l'interconnexion des systèmes ouverts (Open System Interconnection Reference Model). OSI se compose de 7 couches et a été conçu en référence à la stratification de TCP/IP, mais il n'y a donc pas de correspondance précise entre les quatre couches et les sept couches :

OSI décompose les choses en trop de détails au-dessus des quatre couches, et lorsque TCP/IP est appliqué dans la pratique, la gestion des sessions, la conversion de l'encodage, la compression, etc., sont étroitement liées aux applications spécifiques, il est donc difficile de les séparer, c'est pourquoi les couches 5 et 6 ont disparu.

| Couche | Protocoles |
| :--- | :--- |
| [Couche Application](application-layer.md) | [DHCP](application-layer.md#dhcp), [HTTP](application-layer.md#http), [HTTPS](application-layer.md#https), RTMP, P2P, [DNS](application-layer.md#dns), GTP, RPC |
| [Couche Transport](transport-layer.md) | [UDP](transport-layer.md#udp), [TCP](transport-layer.md#tcp) |
| [Couche Réseau](network-layer.md) | [ICMP](network-layer.md#icmp), [IP](network-layer.md#ip), [OSPF](network-layer.md#ospf), [BGP](network-layer.md#bgp), IPSec, GRE |
| [Couche Liaison de Données](data-link-layer.md) | [ARP](data-link-layer.md#arp), [VLAN](data-link-layer.md#vlan), [STP](data-link-layer.md#stp) |
| [Couche Physique](pysical-layer.md) | [Câbles Réseau](pysical-layer.md#8p-8-c), [Concentrateurs](pysical-layer.md#hub) |

Tout paquet circulant sur le réseau est complet. Il peut y avoir des couches inférieures sans couches supérieures, mais il est absolument impossible d'avoir des couches supérieures sans couches inférieures. Par exemple, pour le protocole TCP, qu'il s'agisse de l'établissement d'une connexion en trois temps, de tentatives de retransmission, tant qu'un paquet doit être envoyé, il doit absolument y avoir les couches IP et MAC.

Pour l'implémentation des protocoles UDP et TCP dans les systèmes d'exploitation, on utilise la [Programmation Socket](transport-layer.md#socket-bian-cheng).