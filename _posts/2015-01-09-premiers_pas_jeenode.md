---
layout: post
kind: post
title: Premiers pas avec les JeeNode
description: Ginette, vas-me chercher mes chaussures, j'vais sortir Ginaude.
tags:
- jeenode
---

Ami lecteur, te voilà en possession d'un JeeNode que [tu as soudé tout seul comme un grand]({% post_url 2015-01-06-souder_jeenode %}), et même si tu es content de toi, tu te dis que quand même çà serait pas mal d'en faire quelque chose d'utile.

Alors avant tout j'ai un aveux à te faire: je t'ai menti.

Oui je sais, c'est mal, et depuis j'ai un peu de mal à dormir.


### L'aveux

Je t'expliquais donc qu'[il suffisait de deux JeeNode pour commencer]({% post_url 2015-01-05-plateforme_jeenode %}), et que tu n'avais pas besoin de [JeeLink](http://www.digitalsmarties.net/products/jeelink). Disons que dans l'absolu c'est pas faux, mais pour tester c'est quand même bien pratique de pouvoir connecter un JeeMachin sur son ordinateur de développement.

Personnellement j'utilise donc un [JeeLink](http://www.digitalsmarties.net/products/jeelink), branché sur mon Mac:

![JeeLink](/img/jeenode/jeelink.jpg)


### Giles Inc.

Allez, je suis pas chien, je vais t'expliquer comment çà marche.

Pour commencer, il faut installer [le dernier SDK Arduino](http://arduino.cc/en/Main/Software); en ce qui me concerne çà sera *Arduino 1.0.6* pour *MacOS X*.

Puis lance l'IDE Arduino, et connecte le JeeLink sur un port USB de ton ordi.

Ensuite va dans `Outils > Port série` et sélectionne `/dev/tty.usbserial-A1014IM4`:

![Port série](/img/jeelink/arduino_01.png)

(si le JeeLink n'est pas détecté, installe [ces drivers](http://www.silabs.com/products/mcu/pages/usbtouartbridgevcpdrivers.aspx))

Sélectionne `Outils > Moniteur série`:

![Moniteur série](/img/jeelink/arduino_02.png)

Une fenêtre s'affiche: choisis `57600 baud` dans le menu déroulant en bas à droite de celle-ci.

Et là, normalement, le JeeLink essaie d'entrer en communication avec toi:

![RF12demo](/img/jeelink/arduino_03.png)

Si ce n'est pas le cas, tappe `?` puis clique sur `Envoyer`. Si çà ne marche toujours pas débranche le JeeLink, rebranche le et relance le Moniteur série. Si çà ne marche toujours pas, envoie **MOUISE** au **7 12 12**.

`¯\_(ツ)_/¯`

Alors, par défaut tous les JeeNode et JeeLink sont flashés avec un programme qui s'appelle, attention tiens-toi bien, le [RF12demo](http://jeelabs.net/projects/jeelib/wiki/RF12demo). Encore un bel exemple que:

    There are only two hard things in Computer Science: cache invalidation and naming things. -- Phil Karlton

Non mais **demo** ? Sérieux ? Alors en ce qui concerne les nodes "capteurs" on va en effet rapidement les flasher avec un autre sketch, mais on va par contre bel et bien garder le sketch RF12demo tel quel au niveau du node maitre. Du coup, **demo** je trouve que c'est assez mal choisis pour du code qui va être le point central de notre petit réseau domestique.

Bon, bref, j'imagine que tout cela est historique, et sinon tu peux jeter un oeil au code source [qui se trouve ici](https://github.com/jcw/jeelib/blob/master/examples/RF12/RF12demo/RF12demo.ino).

Revenons à notre JeeLink qui vient de nous dire plein de choses super intéressantes. Il a commencé par un truc dans ce genre là:

    [RF12demo.7] A i1 g100 @ 915 Mhz

Ce qu'on peut traduire par:

    Wesh cousin, je suis le sketch RF12demo version 7 mon pote. Hey ouais ! Je suis le number 1 du crew 100 et je sent la vibe à 915Mhz.

Ce qu'on peut traduire également par:

    Je suis le node numéro 1 dans le groupe numéro 100 et je communique sur du 915Mhz.

Ensuite on a:

    DF I 795 9

Et çà autant te dire qu'on s'en fout complètement.

Après on a la liste des commandes que l'on peut envoyer au JeeLink pour le configurer ou pour envoyer des packets de test.

On va donc commencer par le configurer correctement, en tappant des commandes dans la zone de saisie en haut de la fenêtre:

    15i

Ca c'est pour dire "tu seras le node numéro 15 mon fils"...

    8b

... "et je t'assure, tu communiques sur du 868Mhz, c'est ce qui était marqué sur l'emballage quand tu es né"...

    212g

... "ah au fait ton nom de famille c'est 212. Bienvenue dans notre famille mon fils!"

Une chose qu'il faut que tu saches, c'est que plus tard, quand ton réseau de JeeNode sera en place, il faudra faire en sorte que seul ton master node connecté au raspberry réponde aux autres nodes. Et il y aura donc une configuration supplémentaire à effectuer sur ton JeeLink de test:

    1c

La traduction étant: "tu sais, tu es un peu spécial, tu seras un JeeLink de test, et pour ne pas interférer avec les autres membres de la famille, je te met en mode **collect**, ce qui veut dire que tu entendras bien ce que disent les autres mais tu ne leur répondra jamais, je sais c'est dûr mais c'est tombé sur toi, c'est comme çà, fait avec".

Pour l'instant ce n'est pas nécessaire.

Voilà, le JeeLink est configuré et gardera ses paramètres bien au chaud dans son EEPROM.

A chaque fois que ce JeeLink reçoit une communication, il l'affiche à l'écran sur une seule ligne, genre comme çà:

    ? 41 247 84 70 252 129 16 100 211 6 129 241 18 230 72 64 65 219 157 180 182

Tu remarqueras le **?** en début de ligne. Cela signifie que notre pote JeeLink n'a rien compris. En gros il y a un appareil pas loin qui cause aussi sur le 868Mhz mais soit ce n'est pas un JeeNode, soit c'est un JeeNode modifié qui ne parle plus le même language que ce qu'attend le sketch RF12demo.

Si tu vois ce genre de choses apparaitre:

    OK 4 1 129 166 0

Le **OK** veut dire que le JeeLink a reçu un packet d'un autre JeeNode qu'il comprend.


### Live on RFM12B

Alors, arrêtons nous un instant sur la façon dont ce réseau de JeeNode fonctionne.

Tu l'as compris, il y a une histoire de groupes. On peut avoir 212 groupes de 32 nodes chacun.

Dans mon cas de figure, qui est je te le rappelle la mise en place de capteurs un peu partout chez moi, un node maitre (le node **0**) est branché en permanence sur un raspberry pi pour faire tourner le célèbre sketch RF12demo.

Les autres nodes, numérotés **de 1 à 31**, font tourner un sketch spécifique aux capteurs qu'ils possèdent et envoient leurs données à intervalle régulier. Je configure toujours ces nodes pour qu'ils demandent un acquittement; le node maitre doit est le seul à envoyer ces acquittements. C'est pour ca que mon JeeLink de test est configuré en mode **collect**, pour ne pas interférer avec le node maitre.

Dans l'exemple ci-dessous, on voit le node maitre `JeeNode 0` qui reçoit des données des deux JeeNode `JeeNode 1` et `JeeNode 2` possédant chacun un capteur de température, et il leur répond en envoyant un acquittement.

          +----+      +----+
          |Temp|      |Temp|
          +----+      +----+
        +-+----+--+ +-+----+--+
        |JeeNode 1| |JeeNode 2|
        +----+----+ +-+-------+
             |        |
         temp| ^  temp| ^
             | |      | |
             | |      | |
             v |ack   v |ack
               |        |
           +---+--------+-+
           |  JeeNode 0   |
           | +----------+ |
           | |RFM12demo | |
           | +----------+ |
           +--------------+
                  |
                  |
                  v
          +-----------------+
          |  Raspberry Pi   |
          |  +-----------+  |
          |  |RFM12demo  |  |
          |  |decoder    |  |
          |  +-----------+  |
          +-----------------+
                                                                    
Si les nodes ne recoivent pas d'acquittement, il réémettrons plusieurs fois avant d'abandonner.

Soyons clair, le node maitre ne fait que transmettre les données brutes au raspberry. Il faut donc un programme qui décode les paquets reçus; c'est le role du `RFM12demo decoder` sur le schéma. Notre ami Jean-Claude a développé un outil pour jouer ce role: [HouseMon](http://jeelabs.net/projects/housemon/wiki). Alors autant te dire tout de suite, j'ai installé une vieille version il y a bien longtemps qui ne m'a pas vraiment emballé. Depuis çà a pas mal bougé, mais çà m'a l'air bien compliqué avec des histoires de [jeebus](https://github.com/jcw/jeebus) et de [flow](https://github.com/jcw/flow)... mMmm ok bon, ba *"çà ne me plait pas donc je vais coder un truc à ma façon" (tm)*.

Mais je vois qu'une question te taraude. Que ce passe-t-il si on veut installer plus de 31 nodes chez soit ? Hey bien mon jeune ami, il faut mettre en place un deuxième groupe, et donc un deuxième node maitre pour ce groupe. On se retrouvera alors avec deux nodes maitres sur le raspberry pi, et le `RFM12demo decoder` devra prendre çà en compte.


### Bééééé !

Revenons maintenant à nos moutons, ou plutôt à notre JeeLink de test. Essayons donc de le faire communiquer avec le JeeNode que [tu as soudé]({% post_url 2015-01-06-souder_jeenode %}).

Allez, branche ton JeeNode sur ton ordinateur. Vas-y essaie. Tu n'y arrives pas ? Ahah je suis taquin: il te faut un un [USB BUB](http://www.digitalsmarties.net/products/usb-bub) pour relier le port FTDI du JeeNode au port USB de ton ordinateur:

![USB HUB](/img/jeenode/usb_bub.jpg)

Donc maintenant tu peux y aller, branche ton JeeNode:

![Jeenode avec USB HUB](/img/jeenode/usb_bub_con.jpg)

Les leds **TX** et **RX** du USB BUB vont clignoter un peu.

Ensuite, dans l'IDE Arduino, va dans `Outils > Port série` et sélectionne quelque chose comme `/dev/tty.usbserial-AH019ZSM`:

![Jeenode avec USB HUB connecte](/img/jeelink/arduino_04.png)

Et enfin sélectionne `Outils > Moniteur série`.

Si rien ne s'affiche, c'est peut etre un soucis de connexion, essaie de bouger un peu le JeeNode au niveau du connecteur FTDI. Si vraiment çà ne fonctionne pas, vérifie toutes les soudures de ton JeeNode.

Normalement tu dois voir le JeeNode causer avec toi comme le faisait le JeeLink. C'est normal, c'est parce qu'il a été flashé avec le même sketch `RFM12demo`. Ca va nous permettre de le configurer correctement, cette fois-ci en lui donnant le numéro **16**:

    16i
    8b
    212g

Et voilà. Si tu as bien laissé le JeeLink connecté également, on va pouvoir lui dire bonjour en tappant:

    15a

Ce qui veut dire "Envoi un message de test au node 15 et demande un acquittement".

Ensuite on voit çà:

    -> 0 b

Ce qui veut dire "Ok, je viens d'envoyer un message tout vide".

Et surtout, on recoit ce message:

    OK 143

Eh bien ca mon jeune ami, c'est le JeeLink qui envoie un acquittement, ce qui signifie qu'il a bien reçu ton message. Et là, normalement, tu pleures de joie tellement c'est beau. Comment çà non ? Ah ok... hey ba moi non plus hein.

![I shall not cry](/img/meme/meme_crying.png)

Si jamais tu ne reçois pas la réponse du JeeLink, vérifie que celui-ci n'est pas en mode **collect**.

