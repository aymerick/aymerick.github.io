---
layout: post
kind: post
title: Premiers pas avec les JeeNode
description: Ginette, vas-me chercher mes chaussures, j'vais sortir Ginaude.
tags:
- jeenode
---

Ami lecteur, te voilà en possession d'un JeeNode que [tu as soudé tout seul comme un grand]({% post_url 2015-01-06-souder_jeenode %}), et même si tu es content de toi, tu te dis que quand même çà serait pas mal d'en faire quelque chose.

Alors avant tout j'ai un aveux à te faire: je t'ai menti.

Oui je sais, c'est mal, et depuis j'ai un peu de mal à dormir.


### L'aveux

Je t'expliquais donc qu'[il suffisait de deux JeeNode pour commencer]({% post_url 2015-01-07-premiers_pas_jeenode %}), et que tu n'avais pas besoin de [JeeLink](http://www.digitalsmarties.net/products/jeelink). Disons que dans l'absolu c'est pas faux, mais pour tester c'est quand même sympa d'avoir un JeeNode qui se connecte sur son ordinateur de développement.

Personnellement j'utilise donc un [JeeLink](http://www.digitalsmarties.net/products/jeelink), branché sur mon Mac:

![JeeLink](/img/jeenode/jeelink.jpg)


### Giles Inc.

Pour commencer, il faut installer [le dernier SDK Arduino](http://arduino.cc/en/Main/Software); en ce qui me concerne çà sera *Arduino 1.0.6* pour *MacOS X*.

Lance l'IDE Arduino, puis connecter le JeeLink sur un port USB de ton ordi.

Ensuite va dans `Outils > Port série` et sélectionne `/dev/tty.usbserial-A1014IM4`:

![Port série](/img/jeelink/arduino_01.png)

(si le JeeLink n'est pas détecté, installe [ces drivers](http://www.silabs.com/products/mcu/pages/usbtouartbridgevcpdrivers.aspx))

Puis sélectionne `Outils > Moniteur série`:

![Moniteur série](/img/jeelink/arduino_02.png)

Une fenêtre s'affiche, sélectionne `57600 bauds` dans le menu déroulant en bas à droite de celle-ci.

Et là, normalement, le JeeLink essaie d'entrer en communication avec toi:

![RF12demo](/img/jeelink/arduino_03.png)

Si ce n'est pas le cas, tappe `?` puis clique sur `Envoyer`. Si çà ne marche toujours pas débranche le JeeLink, rebranche le et relance le Moniteur série. Si çà ne marche toujours pas, n'essaie surtout pas de me contacter `¯\_(ツ)_/¯`.

Alors, par défaut tous les JeeNode et JeeLink sont flashés avec un programme qui s'appelle, attention tenez-vous bien, le [RF12demo](http://jeelabs.net/projects/jeelib/wiki/RF12demo). Encore un bel exemple que:

    There are only two hard things in Computer Science: cache invalidation and naming things. -- Phil Karlton

... demo ?? wtf....

@todo

    Current configuration:
     A i1 g100 @ 915 MHz
    > 15i
     O i15 g100 @ 915 MHz
    > 8b
     O i15 g100 @ 868 MHz
    > 1c
     O i15* g100 @ 868 MHz
    > 212g
     O i15* g212 @ 868 MHz

@todo

    ? 41 247 84 70 252 129 16 100 211 6 129 241 18 230 72 64 65 219 157 180 182


### @todo

![USB HUB](/img/jeenode/usb_bub.jpg)
