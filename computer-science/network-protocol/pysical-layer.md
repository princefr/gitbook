# Couche physique

## 8P8C

Également connu sous le nom de **RJ45**, c'est un connecteur de prise couramment utilisé lors de la connexion de câbles Ethernet à paires torsadées. 8P8C (8 positions 8 contacts) signifie qu'il y a 8 positions (Position, se référant à 8 encoches) et 8 contacts (Contact, se référant à 8 points de contact métalliques). C'est ce qu'on appelle généralement un connecteur modulaire ou "connecteur RJ45".

Dans Ethernet 100 mégabits (Fast Ethernet, 10/100M Ethernet), seuls les fils 1, 2, 3 et 6 sont utilisés. Les fils 1 et 2 sont utilisés pour la transmission (TX), tandis que les fils 3 et 6 sont utilisés pour la réception (RX).

Si l'on souhaite connecter directement deux ordinateurs à l'aide de connecteurs RJ45, et que les cartes réseau ne peuvent pas automatiquement s'adapter aux ports d'envoi et de réception, un câble croisé est nécessaire. Cela signifie qu'il faut échanger les positions des fils 1 et 3 d'un côté et les positions des fils 2 et 6 de l'autre côté. Cela permet de transmettre des signaux depuis une extrémité au niveau physique, et que l'autre extrémité puisse les recevoir.

Ensuite, en configurant les adresses IP, les masques de sous-réseau et la passerelle par défaut de ces deux ordinateurs, ils forment le plus petit **réseau local** (**Local Area Network, LAN**).

**Réseau local** (LAN), également appelé **intranet**, est un réseau informatique connectant des ordinateurs dans une zone limitée telle que des résidences, des écoles, des laboratoires, des campus universitaires ou des immeubles de bureaux.

## Hub

Un **concentrateur** (hub Ethernet) est un dispositif qui connecte plusieurs câbles Ethernet à paires torsadées ou à fibres optiques sur le même segment physique.

Fonctionnant entièrement au niveau physique du modèle OSI, il permet à tous les périphériques connectés de travailler dans le même segment réseau. Un concentrateur dispose de plusieurs ports d'entrée/sortie (I/O), et les signaux entrant par l'un quelconque des ports sont répétés à tous les autres ports. Il régénère ou amplifie les signaux numériques qu'il reçoit et les copie sur tous les autres ports.