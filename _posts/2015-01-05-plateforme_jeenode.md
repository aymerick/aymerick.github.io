---
layout: post
kind: post
title: La plateforme jeenode
description: En ce début d'année 2015, installons des JeeNode partout dans la maison.
tags:
- jeenode
---

Bien le bonjour.

En ce début d'année 2015, installons des [JeeNode](http://jeelabs.net/projects/hardware/wiki/JeeNode) partout dans la maison.

![Jeenode](/img/jeenode_solder/jeenode_01.jpg)

Et là je vous vois venir gros comme une maison:

    Ok Aymerick, nous on veut bien hein, on n'est pas contre, mais qu'est ce que c'est qu'un Ginaude ? Et pourquoi qu'on ferait çà, crévindiou de nom de diou de bon diou ?

Héhé, amical habitant de la campagne. Ne t'inquiète pas, je vais tout t'expliquer.


### Le Ginaude

Un [JeeNode](http://jeelabs.net/projects/hardware/wiki/JeeNode) est un circuit électonique créé par [Jean-Claude Wippler](http://jeelabs.org/about/), qui avait comme objectif de créer un réseau local de capteurs communiquant sans-fil. Il est compatible avec la plateforme [arduino](http://www.arduino.cc), on peut donc utiliser les mêmes outils de développement que pour un [arduino](http://www.arduino.cc) standard.

Mais arrêtons-nous un instant sur la vie de [Jean-Claude Wippler](http://jeelabs.org/about/).


### Jean-Claude, sa vie, son oeuvre

Mais qui est vraiment Jean-Claude ? C'est la question que tout le monde se pose. Vivant actuellement aux Pays-Bas, il se dit être à la fois Français, Allemand et Néerlandais tant il est vrai que déjà tout petit il aimait picorer des morceaux de goudat entre deux bières allemandes lors de ses vacances dans un camping de la cote d'azur.

Informaticien indépendant depuis plus de 20 ans, il se lance dans l'électronique pour le fun, et commence à partager ses aventures sur son blog [jeelabs.org](http://jeelabs.org/2008/10/) en 2008. Etant moi-même une quiche en électronique, j'ai parcouru ses **nombreux** billets et j'ai trouvé ses expérimentations fascinantes. Toujours est-il que quelques mois plus tard, Jean-Claude nous sort déjà son [premier JeeNode](http://jeelabs.org/2009/02/13/its-called-a-jeenode/), qu'il déclinera jusqu'à l'actuelle v6 de l'engin.

Un [JeeNode](http://jeelabs.net/projects/hardware/wiki/JeeNode) est doté d'une puce RFM12B pour communiquer en RF. C'est sûr que comparé au wifi, le RFM12B c'est plutôt low-tech. Mais c'est moins cher, çà consomme très peu, et c'est grandement suffisant croyez moi.

Vous trouverez plus d'informations techniques [sur cette page](http://jeelabs.net/projects/hardware/wiki/JeeNode).

JC fini par créer une [boutique en ligne](http://www.digitalsmarties.net/collections/all) (qui est maintenant sous-traitée en Angleterre) et décline sa plateforme avec d'autres types de nodes:

  - Le [JeeNode SMD](http://www.digitalsmarties.net/products/jeenode-smd): une version pré-assemblée du JeeNode
  - Le [JeeNode USB](http://www.digitalsmarties.net/products/jeenode-usb): un JeeNode pré-assemblé avec un port USB à la place du port FTDI
  - Le [JeeLink](http://www.digitalsmarties.net/products/jeelink): un JeeNode au format *clef USB*. On le branche sur un ordi et il joue le role de chef d'orchestre: c'est lui qui recoit les données des autres JeeNodes. Il n'a pas de ports, il est enfermé dans sa boite en plastique.
  - Le [JeeNode Micro](http://www.digitalsmarties.net/products/jeenode-micro): bon ba... un p'tit JeeNode quoi
  - Le [LED Node](http://www.digitalsmarties.net/products/led-node-v2): un JeeNode assez particulier, pour piloter un bandeau de LEDs

Comme tu peux le voir ami lecteur, Jean-Claude est productif. Mais ce n'est pas fini.

Un JeeNode possède [4 ports](http://jeelabs.net/projects/hardware/wiki/JeeNode#Ports-1-4). C'est comme çà, c'est le standard défini par Jean-Claude pour les entrées-sorties et l'alimentation. Et du coup, toujours sur [la boutique de jeelabs](http://www.digitalsmarties.net/collections/all), on trouve tout un tas de *plugs* à brancher sur nos JeeNode:

  - Le [Room Board](http://www.digitalsmarties.net/products/room-board): tout ce qu'il faut pour mesurer la température, l'humidité, la luminosité et détecter des mouvements. Bon, le capteur HYT131 pour l'humidité coûte un bras, mais il y a moyen de le remplacer par un DHT22 beaucoup plus abordable.
  - Le [Dimmer Plug](http://www.digitalsmarties.net/products/dimmer-plug): bon ba... pour dimmer
  - Le [Gravity Plug](http://www.digitalsmarties.net/products/gravity-plug): rien à voir avec le film... c'est juste un accéléromètre
  - Le [Infrared Plug](http://www.digitalsmarties.net/products/infrared-plug): devinez
  - ...

Il y a de quoi faire plein de trucs sympatoches.

![Jeenode with Room Board](/img/jeenode/jeenode_roomboard.jpg)

Dans sa grande bonté, Jean-Claude a mis toute sa plateforme en open source, et çà c'est essentiel. Imaginons un instant que Jean-Claude se fasse renverser par une meule de gouda géante alors qu'il écoutait du Eddy Mitchell lors de la fête de la bière. PAF ! Sa boutique en ligne ferme, et maintenant comment je fais moi si je veux ajouter d'autres JeeNode chez moi ? Eh bien je prend les schémas électronique fournis par feu Jean-Claude, et je les fabrique moi-même, via [Seeed Studio](http://www.seeedstudio.com/service/index.php?r=pcb) par exemple.


### Oui ? Et donc ?

La plateforme [JeeNode](http://jeelabs.net/projects/hardware/wiki/JeeNode) correspond parfaitement à mon besoin car:

  - je veux mettre des capteurs de température/humidité/... partout dans ma maison, à pas trop cher
  - je n'ai pas envie de changer les piles tous les 2 mois (tous les 2 ans c'est mieux)
  - je ne veux pas être dépendant d'un matériel propriétaire fermé
  - je n'y connais rien en électronique, mais çà ne me gêne pas de souder des trucs, voir çà m'amuse

Alors c'est parti mon kiki.

![Jeenode hidden in an Ikea Ribba](/img/jeenode/jeenode_ribba.jpg)


### Achète moi un Ginaude Ginette !

Coté serveur, un raspberry pi fait l'affaire:

  - un [Raspberry Pi B+](http://www.raspberrypi.org) acheté chez [RadioSpare Particuliers](http://www.rs-particuliers.com/WebCatalog/Raspberry_Pi_B_-8111284.aspx) (le week-end avec frais de ports offerts): 31,26€
  - une [alim micro USB](http://www.amazon.fr/gp/product/B00IMU7TF4): 8,49€
  - une [carte micro SD](http://www.amazon.fr/gp/product/B00MWXUKDK): 8,90€

Et avec deux nodes pour débuter:

  - deux [JeeNode](http://www.digitalsmarties.net/products/jeenode): 18,50€ * 2
  - un [USB BUB](http://www.digitalsmarties.net/products/usb-bub): 13,50€
  - un [boitier pour 3 piles AA](http://www.dx.com/p/4-5v-3-x-aa-battery-holder-case-box-with-leads-103858#.VKqhd2SG_50): 1,50€

Un seul [USB BUB](http://www.digitalsmarties.net/products/usb-bub) suffit: il permet de raccorder un JeeNode à un ordi pour mettre à jour le JeeNode. Dans les faits, il ne servira qu'une fois par JeeNode (en dehors de la phase de conception et de test des programmes).

On n'est pas obligé d'acheter un JeeLink: un simple JeeNode peut servir de node principal en le branchant directement sur le GPIO du raspberry pi [comme expliqué ici](http://jeelabs.org/2012/09/20/serial-hookup-jeenode-to-raspberry-pi/).

![Jeenode](/img/jeenode/jeenode_rpi.jpg)


### Et après j'fais quoi ?

Houla comme tu es pressé. Ca va venir ne t'inquiète pas, tu auras la suite bientôt. En attendant si tu t'ennuies tu peux toujours lire [les billets de blog de Jean-Claude](http://jeelabs.org/). Je sais c'est touffu et c'est pas facile de s'y retrouver mais ca vaut le coup.

Allez, je t'embrasse, à la prochaine.


### Références

- <http://jeelabs.net/projects/hardware/wiki/JeeNode>
- <http://www.digitalsmarties.net/collections/all>
