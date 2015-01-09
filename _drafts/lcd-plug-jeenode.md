---
layout: post
kind: post
title: Le LCD Plug pour JeeNode
description: Eh Ginette ! Mais où qu't'as mis heul CD ?
tags:
- jeenode
---

Aujourd'hui, découvrons le [LCD Plug](http://www.digitalsmarties.net/products/lcd-plug) pour JeeNode.

En ce qui me concerne j'ai la version *2×16 LCD (grey/green)*:

![JeeNode LCD Plug](/img/jeenode_lcd/lcd_plug_01.jpg)


### Montage

Parmis ces éléments il y a trois intrus. Sauras-tu les reconnaitre ?

![JeeNode LCD Plug](/img/jeenode_lcd/lcd_plug_02.jpg)

Bien, commençons par le header du port. Coup de bol, c'est un header à angle droit qui est livré avec le kit, donc c'est plus simple. Remarque l'astucieuse utilisation de l'autre header pour qualer la carte, ce qui permet de souder bien droit:

![JeeNode LCD Plug](/img/jeenode_lcd/lcd_plug_03.jpg)

Ensuite on soude la carte sur le LCD, avec le header mâle-mâle:

![JeeNode LCD Plug](/img/jeenode_lcd/lcd_plug_04.jpg)

Et on soude de l'autre coté aussi:

![JeeNode LCD Plug](/img/jeenode_lcd/lcd_plug_05.jpg)

Il faut également shunter l'emplacement pour une résistance marqué **R3**, car comme [expliqué ici](http://forum.jeelabs.net/node/595.html), on n'a pas besoin de résistance avec le LCD fourni dans le kit:

> For the LCD display supplied with the kit and running at 3.3V, you can simply short out the resistor jumper, i.e. 0Ω.

Et cette info vient de [Jean-Claude]({% post_url 2015-01-05-plateforme_jeenode %}) himself, donc on peut y aller les yeux fermés:

![JeeNode LCD Plug](/img/jeenode_lcd/lcd_plug_06.jpg)

Le montage est maintenant terminé. Les intrus étaient donc le long header mâle-femelle et les deux résistances. Ne me demande pas pourquoi ils étaient fournis avec le kit, je n'en sais rien. Voilà, tu peux les jeter. Ou les donner à ta petite soeur. On s'en fout.


### Test

Relie le LCD Plug au **port P4** du JeeNode en faisant bien attention au sens; si tu veux mon avis tu ferais mieux de mettre le fil rouge pour relier les deux **P** (Power), mais bon, tu fais comme tu le sents hein:

![JeeNode with LCD Plug](/img/jeenode_lcd/lcd_plug_07.jpg)

Maintenant branche le JeeNode sur ton ordi via le USB BUB, lance l'IDE Arduino, puis sélectionne le JeeNode dans `Outils > Port série`.

Il ne te reste plus qu'à ouvrir le sketch qui se trouve dans `Fichier > Exemples > jeelib > Ports > lcd_demo` et à l'uploader sur le JeeNode:

![LCD Plug on](/img/jeenode_lcd/lcd_plug_08.png)

Et là, incroyable mais vrai, y'a des machins qui s'affichent sur le LCD:

![LCD Plug on](/img/jeenode_lcd/lcd_plug_09.jpg)

Si l'écran s'allume mais rien ne s'affiche, équipe toi d'un petit tournevis et règle le *truc* à l'arrière du LCD Plug:

![LCD Plug on](/img/jeenode_lcd/lcd_plug_10.jpg)

Et voilà, maintenant tu peux afficher des caractères sur un petit écran relié à un JeeNode. Si ça c'est pas le plus beau jour de ta vie j'y connais rien en plus beaux jours de vie. Hein mon ami ?

Allez, je vois que tu es tout ému, et c'est bien normal. Je te laisse donc te remettre de tes émotions et te dis à bientôt, car nous avons encore d'autres grands moments à passer ensemble.

La bise Élise.


### References

  - <http://jeelabs.net/projects/hardware/wiki/LCD_Plug>
  - <http://jeelabs.org/2009/11/01/lcd-plug/>
  - <http://forum.jeelabs.net/node/595.html>
  - <http://edvoncken.net/2011/12/assembling-the-jeelabs-lcd-plug/>
