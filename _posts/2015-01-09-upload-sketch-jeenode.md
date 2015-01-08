---
layout: post
kind: post
title: Upload de sketch sur un jeenode
description: Ah ma Ginette ! Qu'est ce qu'on rigole avec toi !
tags:
- jeenode
---

Ami jeenodeur, bien le bonjour.

Aujourd'hui ta mission sera d'uploader un nouveau sketch dans ton [magnifique JeeNode]({% post_url 2015-01-06-souder_jeenode %}) pour remplacer le [sketch RF12demo]({% post_url 2015-01-08-premiers_pas_jeenode %}).

Pour rappel, par *sketch* je ne parle pas du jeu d'acteur dont le but est de te faire hurler de rire, comme par exemple celui-ci:

<iframe frameborder="0" width="480" height="270" src="//www.dailymotion.com/embed/video/x3q5gc" allowfullscreen></iframe><br /><a href="http://www.dailymotion.com/video/x3q5gc_les-nuls-professeur-thibault-la-mou_fun" target="_blank">Les Nuls - Professeur Thibault - La mouche qui...</a> <i>par <a href="http://www.dailymotion.com/les-nuls" target="_blank">les-nuls</a></i>

Non non non, dans l'univers *Arduino*, un sketch est la façon de dire *programme*. Je ne sais absolument pas d'où ça vient et pourquoi ils ont choisi ce nom là. Peut-être qu'à la base c'était une énorme blague, j'en sais rien, je ne veux pas savoir, laisse moi tranquille, merci.

Ah, alors on me fait signe dans mon oreillette qu'en français on dit **croquis**, du coup oublie tout ce que je viens de dire, nous allons donc écrire un croq... ho non c'est trop naze, on va garder *sketch*.


### Alors c'est l'histoire d'un Italien, un Allemand et un Belge...

Pour commencer, il faut installer la [jeelib](http://jeelabs.net/projects/jeelib/wiki), qui est la lib de base pour les JeeNode. Sur MacOS, c'est comme ça que ça se passe:

    $ cd ~/Documents/Arduino/libraries/
    $ git clone https://github.com/jcw/jeelib

Puis relance l'IDE Arduino.

Ensuite va dans `Ficher > Exemples > jeelib > RF12 > radioBlip`:

![Sketch radioBlip](/img/jeenode_sketch/sketch_radioBlip.png)

Une fenêtre s'ouvre avec le sketch `radioBlip`. Celui-ci envoie un message toutes les 60 secondes, avec comme payload un compteur qui s'incrémente. Nous allons le modifier un peu.

Tout d'abord, remplace la ligne:

    rf12_initialize(22, RF12_868MHZ, 5);

par celle-ci:

    rf12_initialize(16, RF12_868MHZ, 212);

En effet, dans ce sketch le numéro de node et le groupe sont en dûr. C'est mal mais on va vivre avec. On le modifie juste pour mettre les bonnes valeurs: c'est le node **16** du groupe **212** qui fera tourner ce sketch.

Puis, on va faire en sorte d'envoyer des messages plus souvent, genre toutes les 10 secondes. Change cette ligne:

    Sleepy::loseSomeTime(60000);

par:

    Sleepy::loseSomeTime(10000);

Sauvegarde le fichier ainsi modifié.

Clique maintenant sur le bouton `Vérifier` pour compiler le sketch. Si tout va bien, le message **Compilation terminée** s'affiche sans erreur. Sinon, eh ba... bon courage:

![Sketch check](/img/jeenode_sketch/sketch_check.png)

Maintenant on va uploader le sketch. Branche ton JeeNode sélectionne le dans `Outils > Port série` (genre `/dev/tty.usbserial-AH019ZSM`).

**Fait bien attention à ne pas sélectionner le [JeeLink de test]({% post_url 2015-01-08-premiers_pas_jeenode %}) si tu l'as laissé branché sur ton ordi.**

Ensuite clique sur la deuxième icone qui s'appelle... QUOI?!... non mais... qu'est-ce que c'est que ce.... ouaw... ok. Bon, donc cette deuxième icône s'appelle `Téléverser`, qui permet donc de... téléverser un croquis. Voilà voilà. On n'a pas peur du ridicule, non non.

Et donc, si le *téléversement* foire lamentablement avec le message `avrdude: stk500_getsync(): not in sync: resp=0xfe` c'est que nous sommes face à un **problème de drivers! TADAAAAAAAAA!**

![Sketch upload fail](/img/jeenode_sketch/sketch_upload_fail.png)

Oui ça fait toujours peur, mais ne t'inquiète pas j'ai la solution: rend toi [sur cette page](http://www.ftdichip.com/Drivers/VCP.htm) et télécharge le fichier qui correspond à ta machine. Par exemple en ce qui me concerne j'ai besoin du fichier *Mac OS X 2.2.18* pour *x64 (64-bit)*.

Ensuite on installe les drivers, on débranche puis rebranche le USB HUB, et on retente le... *téléversement* donc (non je ne m'y ferai pas, c'est pas possible). Et là, maintenant, ça doit est bon:

![Sketch upload ok](/img/jeenode_sketch/sketch_upload_ok.png)

*"Hey mais c'est super !"* te dis-tu, *"Mais... et maintenant ? Qu'est-ce que quoi ?"*. Alors dèjà tu te calmes, tu respires à fond, et tu me laisses continuer.

Branche donc ton JeeLink de test, et sélectionne le dans `Outils > Port série` (ex: `/dev/tty.usbserial-A1014IM4`), puis affiche `Outils > Moniteur série`. Voyons-voir ce que reçoit notre ami JeeLink.

Tu dois voir apparaitre un nouveau message toutes les 10 secondes en provenance du node 16, avec un compteur qui s'incrémente:

    OK 16 1 0 0 0
    OK 16 2 0 0 0
    OK 16 3 0 0 0
    OK 16 4 0 0 0

Wouhou ! Trop la fête ! Tu viens d'uploader un sketch que tu as modifié sur ton JeeNode, et ça fonctionne ! Tu trouves pas ça génial ? ... Comment ça bof ? Il t'en faut plus pour être impressionné ?

Okaaayyy, défi accepté.

![Challenge accepted](/img/meme/challenge_accepted.jpg)

On se retrouve bientôt pour t'en mettre plein les mirettes.

Repose toi bien mon ami, car je vais avoir besoin de toute ton attention la prochaine fois.

A+ Arthus.
