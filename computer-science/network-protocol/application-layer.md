# Couche application

## DHCP

Le **Protocole de Configuration Dynamique des Hôtes** (**DHCP**, pour *Dynamic Host Configuration Protocol*) est un protocole réseau utilisé dans les réseaux locaux qui fonctionne avec le protocole UDP. Il a principalement deux objectifs :

- Attribution automatique des adresses IP aux utilisateurs dans un réseau interne ou par un fournisseur de services Internet.
- Moyen pour les administrateurs de réseau interne de gérer de manière centralisée tous les ordinateurs, par exemple via PXE.

Le DHCP est une amélioration de BOOTP.

### Découverte (*Discover*)

Lorsqu'une nouvelle machine A rejoint le réseau, elle ne connaît que son adresse MAC. Elle doit donc diffuser une demande pour obtenir une adresse IP.

|    En-tête    |                              Contenu                             |
| :-----------: | :---------------------------------------------------------------: |
| En-tête MAC   |      <p>MAC de A</p><p>Diffusion MAC (ff:ff:ff:ff:ff:ff)</p>      |
| En-tête IP    | <p>IP de A : 0.0.0.0</p><p>Diffusion IP : 255.255.255.255</p>    |
| En-tête UDP   |              <p>Port source : 68</p><p>Port destination : 67</p>              |
| En-tête BOOTP |                         Boot request                        |
|               |                      Voici mon adresse MAC, je n'ai pas encore d'IP                     |

### Offre (*Offer*)

Le serveur DHCP reçoit cette demande et constate que la machine A n'a pas d'adresse IP, il lui attribue donc une adresse IP.

|    En-tête    |                                Contenu                               |
| :-----------: | :------------------------------------------------------------------: |
| En-tête MAC   |      <p>MAC du serveur DHCP</p><p>Diffusion MAC (ff:ff:ff:ff:ff:ff)</p>      |
| En-tête IP    | <p>IP du serveur DHCP : 192.168.1.2</p><p>Diffusion IP : 255.255.255.255</p> |
| En-tête UDP   |                  <p>Port source : 67</p><p>Port destination : 68</p>                  |
| En-tête BOOTP |                           Boot reply (Réponse de boot)                          |
|               |                Voici ton adresse MAC, je t'ai attribué cette adresse IP, que penses-tu ?               |

### Requête (*Request*)

La machine A peut recevoir des réponses de plusieurs serveurs DHCP, elle choisira généralement la première arrivée et enverra une demande DHCP en diffusion, contenant l'adresse MAC du client, l'adresse IP de la location acceptée, l'adresse du serveur DHCP qui a offert la location, et informera les autres serveurs DHCP qu'elle accepte cette offre et leur demande de retirer leurs offres.

Comme la confirmation finale du serveur DHCP n'a pas encore été obtenue, le client utilise toujours 0.0.0.0 comme adresse IP source et 255.255.255.255 comme adresse de destination pour diffuser.

|    En-tête    |                              Contenu                             |
| :-----------: | :---------------------------------------------------------------: |
| En-tête MAC   |      <p>MAC de A</p><p>Diffusion MAC (ff:ff:ff:ff:ff:ff)</p>      |
| En-tête IP    | <p>IP de A : 0.0.0.0</p><p>Diffusion IP : 255.255.255.255</p>    |
| En-tête UDP   |              <p>Port source : 68</p><p>Port destination : 67</p>              |
| En-tête BOOTP |                         Boot request                        |
|               |            Voici mon adresse MAC, je veux louer cette adresse IP du serveur DHCP         |

### Confirmation (*ACK*)

Le serveur DHCP envoie un message de confirmation DHCP (*ACK*) au client.

|    En-tête    |                                Contenu                               |
| :-----------: | :------------------------------------------------------------------: |
| En-tête MAC   |      <p>MAC du serveur DHCP</p><p>Diffusion MAC (ff:ff:ff:ff:ff:ff)</p>      |
| En-tête IP    | <p>IP du serveur DHCP : 192.168.1.2</p><p>Diffusion IP : 255.255.255.255</p> |
| En-tête UDP   |                  <p>Port source : 67</p><p>Port destination : 68</p>                  |
| En-tête BOOTP |                           Boot reply (Réponse de boot)                          |
|               |                                  DHCP ACK (ACK DHCP)                                  |

### Diffusion du client

Lorsque le bail est finalement conclu, une diffusion est encore nécessaire pour informer tout le monde.

### Renouvellement et recyclage

Lorsque le bail arrive à expiration, l'administrateur doit récupérer l'IP.

Le client enverra une demande DHCP au serveur DHCP pour renouveler le bail lorsque 50 % du bail est écoulé. Le client reçoit la réponse du serveur DHCP contenant un nouveau bail ainsi que d'autres paramètres TCP/IP mis à jour. Ainsi, la mise à jour de la location d'IP est effectuée.

## PXE

Le protocole DHCP peut installer un système d'exploitation pour les clients, ce qui est très utile dans le domaine du cloud computing.

## HTTP

Le protocole HTTP est un protocole de transfert de données entre un navigateur et un serveur. Fondamentalement, il s'agit d'un format de communication convenu entre un navigateur et un serveur.

Le protocole HTTP est basé sur TCP. La version 1.1 du mode HTTP active Keep-Alive (Connection

: keep-alive), ce qui permet de réutiliser une connexion TCP pour plusieurs requêtes HTTP, sans avoir à refaire la procédure de connexion à chaque fois.

![Format de requête HTTP](<../../.gitbook/assets/image (171).png>)

- **Méthode** : GET, POST, PUT, DELETE, HEAD, PATCH, etc.
- **Version** : 1.0, 1.1, 2.0
- En-têtes :
  - Accept-Charset : Jeu de caractères accepté par le client
  - Content-Type : Format du corps du message
  - **Cache-Control** : si max-age est à 0, alors pas de mise en cache ; sinon, si le temps de mise en cache des ressources est inférieur à max-age, elles peuvent être mises en cache.
  - If-Modified-Since : si la ressource n'a pas été mise à jour depuis un certain temps, le client n'a pas besoin de télécharger la ressource la plus récente.

![Format de réponse HTTP](<../../.gitbook/assets/image (198).png>)

Le protocole HTTP est sans état, ce qui signifie que l'application Web ne sait pas d'où provient cette demande.

### Cookie

Le cookie est un en-tête de requête HTTP. Les applications Web peuvent stocker des informations utilisateur dans les cookies, et chaque demande de l'utilisateur comprendra l'en-tête Cookie. **Les cookies sont essentiellement des fichiers stockés localement sur l'ordinateur de l'utilisateur, et ces informations sont envoyées à chaque demande.**

### Session

Si les informations utilisateur sont stockées en clair dans les cookies, cela pose des problèmes de sécurité. Par conséquent, le serveur conserve l'état de l'utilisateur, renvoie l'identifiant de session au client, et le client envoie cet identifiant de session dans chaque demande via un cookie.

## HTTPS

## DNS

Les adresses IP sont difficiles à retenir, c'est pourquoi nous avons des noms de domaine. Les noms de domaine sont composés de plusieurs mots séparés par des points, avec le *domaine de premier niveau* à l'extrême droite, suivi du *domaine de deuxième niveau*, et ainsi de suite vers la gauche. Le plus à gauche est généralement utilisé pour indiquer l'utilisation de l'hôte, comme www pour le World Wide Web, ou mail pour le courrier électronique. Tous les noms de domaine se terminent par (.) mais cela est généralement omis. Les serveurs DNS écoutent généralement sur le port 53.

Le DNS est responsable de la résolution des noms de domaine en adresses IP. Son système central est un service distribué en forme d'arbre à trois niveaux :

1. Serveur de noms de domaine racine (*Root DNS Server*) : responsable des domaines de premier niveau, tels que le retour des adresses des serveurs de domaine de premier niveau comme com, cn, net, etc. Actuellement, il existe 13 groupes de serveurs de noms de domaine racine, chacun avec des centaines de serveurs miroirs.
2. Serveur de noms de domaine de premier niveau (*Top-level DNS Server*) : responsable des serveurs de noms de domaine d'autorité pour leurs propres domaines de premier niveau, comme le serveur de domaine de premier niveau com qui renvoie l'adresse de apple.com.
3. Serveur de noms de domaine d'autorité (*Authoritative DNS Server*) : responsable de son propre domaine, comme le serveur de domaine d'autorité apple.com qui renvoie l'adresse de www.apple.com.

Les serveurs DNS centraux ne peuvent pas servir tous les internautes du monde entier, c'est pourquoi de nombreuses entreprises et fournisseurs de services Internet ont leurs propres serveurs DNS, qui sont essentiellement des caches de données. Les systèmes d'exploitation conservent également des caches pour la résolution DNS, et en plus de cela, /etc/hosts peut être utilisé pour personnaliser les enregistrements DNS.

Les services DNS prennent en charge plusieurs types d'enregistrements :

- Enregistrement A : utilisé pour convertir un nom de domaine en adresse IP.
- Enregistrement CNAME : utilisé pour créer des alias.
- Enregistrement NS : indique l'adresse du serveur de noms de domaine associé au domaine.

Vous pouvez configurer le serveur de noms de domaine dans /etc/resolve.conf, et utiliser des outils comme nslookup ou dig pour afficher les noms de domaine.