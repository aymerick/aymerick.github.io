---
layout: post
kind: post
title: Le LCD Plug pour JeeNode
description: Eh Ginette ! Mais où qu't'as mis heul CD ?
tags:
- jeenode
---

Aujourd'hui, jouons avec le [LCD Plug](http://www.digitalsmarties.net/products/lcd-plug) JeeNode.

![JeeNode LCD Plug](/img/jeenode_lcd/lcd_plug_01.jpg)

### Montage

Parmis ces éléments il y a trois intrus. Sauras-tu les reconnaitre ?

![JeeNode LCD Plug](/img/jeenode_lcd/lcd_plug_02.jpg)

Bon, commençons par le header de port. Coup de bol, c'est un header droit qui est livré avec le kit, donc c'est plus simple. Remarquez l'astucieuse utilisation de l'autre header pour qualer la carte, ce qui permet de souder bien droit:

![JeeNode LCD Plug](/img/jeenode_lcd/lcd_plug_03.jpg)

Ensuite on soude la carte sur le LCD, avec le header mâle-mâle:

![JeeNode LCD Plug](/img/jeenode_lcd/lcd_plug_04.jpg)

Et on soude de l'autre coté aussi:

![JeeNode LCD Plug](/img/jeenode_lcd/lcd_plug_05.jpg)

Il faut également shunter l'emplacement pour une résistance marqué R3, car comme [expliqué ici](http://forum.jeelabs.net/node/595.html), on n'a pas besoin de résistance avec le LCD fourni dans le kit:

> For the LCD display supplied with the kit and running at 3.3V, you can simply short out the resistor jumper, i.e. 0Ω.

Et cette info vient de [Jean-Claude]({% post_url 2015-01-05-plateforme_jeenode %}) himself, donc on peut y aller les yeux fermés:

![JeeNode LCD Plug](/img/jeenode_lcd/lcd_plug_06.jpg)

Le montage est maintenant terminé. Les intrus étaient donc le long header mâle-femelle et les deux résistances. Ne me demandez pas pourquoi ils étaient fournis j'en sais rien. Voilà, vous pouvez les jeter. Ou les donnez à votre petite soeur. On s'en fout.

@todo


### References

  - <http://jeelabs.net/projects/hardware/wiki/LCD_Plug>
  - <http://jeelabs.org/2009/11/01/lcd-plug/>
  - <http://forum.jeelabs.net/node/595.html>
  - <http://edvoncken.net/2011/12/assembling-the-jeelabs-lcd-plug/>
