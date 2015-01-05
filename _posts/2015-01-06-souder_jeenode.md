---
layout: post
title: Souder un JeeNode
tags:
- jeenode
---

Ah ba tiens! Te revoilà ami lecteur.

J'imagine que suite à mon dernier billet sur [la vie de Jean-Claude]({% post_url 2015-01-05-plateforme_jeenode %}), tu t'es empressé d'acheter un JeeNode, fougeux que tu es.

Et du coup tu te retrouves devant çà:

![Jeenode Kit][1]

Je vois bien ton désarroi. Personne ne t'avais prévenu. Hey ba ouais, il va falloir faire resurgir tes vieux cours d'EMT du passé et souder tout çà.

Alors attention, que çà soit bien clair, on dit bien **souder** et pas **soudre**, parce que si tu dis soudre, tu disparais... hoho... hum passons.

Si jamais tu veux la version originale, tu peux aller sur [le blog de Jean-Claude](http://jeelabs.org/2010/09/26/assembling-the-jeenode-v5/).


### Ces Slaves savent souder sans soucis, si-si

Allez, on y va, on se lance, on y croit, on n'a pas peur.

On est donc devant çà:

![Jeenode Kit][2]

Alors, ma petite astuce, mon petit plus à moi, c'est de remplacer cette **saloperie de header tout pourrit vendu avec le kit** par des headers à angle droit [vendus dans la boutique de Jean-Claude](http://www.digitalsmarties.net/products/right-angle-plug-headers).

Regarde bien; à gauche, le tout pourrit, et à droite, le moins pourrit:

![Jeenode Kit][3]

Je crois que la photo parle d'elle même, c'est flagrant.

Non mais à la base, la soudure c'est pas mon truc, alors forcément si en plus y'a des pièges, çà peut énerver.

Bref, gardons çà pour plus tard, et continuons.

Commençons donc par éliminer la résistance. Met la résistance dans son emplacement **R1**, retourne le tout, et soude les pates:

![Jeenode Kit][4]

Ensuite tu peux couper les pates (admire au passage la mise en scène avec la pince juste à côté):

![Jeenode Kit][5]

Et hop! Une résistance soudée! BRAVO! Tu voies qu'ils servent enfin à quelque chose tes cours d'EMT du collège:

![Jeenode Kit][6]

Ensuite tu fais la même chose avec les 4 petits condensateurs:

![Jeenode Kit][7]

Alors là attention, accroche-toi bien, il y a une difficulté. Nous attaquons l'étape dites du **GROS CONDENSATEUR**. Oui je sais çà fait peur, mais c'est fait exprès. Car le gros condensateur est **polarisé**.

Alors, d'après toi, est-ce que çà veut dire que:

  1. Il est le héro du dernier San Antonio
  2. Il s'est fait prendre au Polaroïd
  3. Il cherche constamment un mec qui s'appellerait Risé mais qui ne serait jamais là
  4. Contrairement à d'autres gros condensateurs, il n'est pas la risé des petits condensateurs
  5. Y'a un sens et faut faire gaffe (en gros)

Oui ok, donc oui y'a un sens et faut faire gaffe: la pate la plus longue est le **+**, et se trouve donc à droite. Voilà c'est tout:

![Jeenode Kit][8]

Passons maintenant au régulateur de tension. La aussi y'a un sens, mais maintenant que tu es bien chaud, je te vois mal te gourer sur celui-là.

Commence donc par tordre la pate du milieu:

![Jeenode Kit][9]

Et sinon, tu connais l'histoire de *Hop! le régulateur* ? Bon va c'est un régulateur qui passe et puis Hop! le régulateur:

![Jeenode Kit][10]

Ensuite, si sur la photo suivante il y a un **cercle rouge**, ce n'est pas parce qu'il faut dessiner un rond au feutre rouge sur le JeeNode. Non pas du tout, c'est juste pour t'indiquer l'endroit où il faut mettre un peu de soudure. Alors oui, avant il y avait une diode, mais Jean-Claude s'est dit que finalement çà sert à rien. Du coup il faut relier les deux machins. Voilà. Et oui, *machin* c'est le terme technique qui convient:

![Jeenode Kit][11]

Attention accroche toi bien, tu vas maintenant enchainer **54 soudures** d'affilée!

D'abord le support du chip:

![Jeenode Kit][12]

Ensuite les 4 headers de port:

![Jeenode Kit][13]

Voilà, tu l'as fait. Tu dois te sentir super fort maintenant.

Je te vois même esquisser un sourire *"LOL trop facile"* quand vient le tour du résonnateur:

![Jeenode Kit][14]

Je te conseille quand même de moins faire le malin, car c'est avec la prochaine étape que tu vas commencer à bien transpirer des doigts.

Non, tu ne rêves pas, tu vas effectivement souder le composant RFM12B comme tu penses que tu dois le faire. Et fait bien gaffe parce qu'il y a un sens, donc regarde bien la photo.

On va commencer par mettre un peu de soudure sur un emplacement du composant, genre en haut à gauche:

![Jeenode Kit][15]

Le but maintenant est de positionner le RFM12B correctement, puis de chauffer la soudure préalablement posée pour la liquéfier. Et quand ca refroidit, le RFM12B doit être bien en place, comme ceci:

![Jeenode Kit][16]

A partir de là tu peux finir de le souder tout partout sauf au niveau du trou qui a un **A** à côté. Celui-là c'est pour l'antenne.

Dénude donc un peu l'antenne, fait la passer dans ce trou, et soude la:

![Jeenode Kit][18]

Enfin, c'est au tout du port FTDI. Et c'est là que tu me remercies pour l'astuce du header à angle droit. Tu vas voir comme c'est facile avec ce header. Si tu es joueur, essaye donc de souder le header fourni avec le kit à la place, puis ouvre la fenêtre et jete le tout dehors en gueulant.

Bon bref, voilà le bon header bien soudé:

![Jeenode Kit][19]

ET TADA ! Tu as soudé ton premier JeeNode, bravo je suis fier de toi:

![Jeenode Kit][20]

Ca mérite même une deuxième photo tiens:

![Jeenode Kit][21]

Donc voilà, c'est fini. Tu peux maintenant exposer ton JeeNode dans ton salon pour bien montrer à tout le monde que tu es un pro en électronique, un vrai. Un de ceux qui savent ce qui se cache derrière des termes techniques tels que **pate** et **machin**.

Sinon, dans un futur plus ou moins proche, je t'expliquerai comment le programmer, lui ajouter des capteurs...

Mais pour l'instant, je te dis à la prochaine, et n'oublie pas d'éteindre ton fer à souder avant de partir.


### Références

- <http://jeelabs.org/2010/09/26/assembling-the-jeenode-v5/>


[1]: /img/jeenode_solder/jeenode_01.jpg
[2]: /img/jeenode_solder/jeenode_02.jpg
[3]: /img/jeenode_solder/jeenode_03.jpg
[4]: /img/jeenode_solder/jeenode_04.jpg
[5]: /img/jeenode_solder/jeenode_05.jpg
[6]: /img/jeenode_solder/jeenode_06.jpg
[7]: /img/jeenode_solder/jeenode_07.jpg
[8]: /img/jeenode_solder/jeenode_08.jpg
[9]: /img/jeenode_solder/jeenode_09.jpg
[10]: /img/jeenode_solder/jeenode_10.jpg
[11]: /img/jeenode_solder/jeenode_11.jpg
[12]: /img/jeenode_solder/jeenode_12.jpg
[13]: /img/jeenode_solder/jeenode_13.jpg
[14]: /img/jeenode_solder/jeenode_14.jpg
[15]: /img/jeenode_solder/jeenode_15.jpg
[16]: /img/jeenode_solder/jeenode_16.jpg
[18]: /img/jeenode_solder/jeenode_18.jpg
[19]: /img/jeenode_solder/jeenode_19.jpg
[20]: /img/jeenode_solder/jeenode_20.jpg
[21]: /img/jeenode_solder/jeenode_21.jpg
