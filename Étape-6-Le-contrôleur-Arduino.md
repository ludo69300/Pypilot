Comme indiqué, le logiciel Pypilot n'est pas complet sans le logiciel de contrôle du moteur, qui doit fonctionner sur un Arduino. À ma connaissance, on utilise toujours un Arduino Nano, la version compacte.

Évitez d'acheter un clone bon marché. Certains paramètres d'un Arduino nécessitent des manipulations complexes pour être correctement réglés. Ces paramètres sont appelés fusibles, et je les compare aux paramètres du BIOS d'un PC. Si vous achetez un Arduino correct, les fusibles sont correctement réglés. Si vous les achetez chez un revendeur spécialisé, vous augmenterez vos chances d'obtenir un Arduino correct. Si vos fusibles sont incorrects, Pypilot affichera un message d'erreur clair.

Il faut ensuite obtenir le croquis. Vous avez besoin de deux fichiers, le mieux étant de les télécharger sur GitHub : https://github.com/pypilot/pypilot/tree/master/arduino/motor. Il s'agit des fichiers motor.ino et crc.h.

Pour transférer le croquis sur l'Arduino, le plus simple est de le faire avec un câble USB. Il existe deux options : programmer l'Arduino sous Linux ou sous Windows.

* La méthode Linux est expliquée dans le fichier readme de GitHub. Elle nécessite un programme appelé avrdude, qui ne se trouve pas sur l'image openplotter. Vous devrez donc l'installer avec la commande « sudo apt-get install avrdude ». N'oubliez pas non plus « sudo apt install arduino ».
* La méthode Windows nécessite le téléchargement et l'installation de l'[IDE Arduino](https://www.arduino.cc/en/software/). Placez les deux fichiers dans un répertoire nommé « motor », double-cliquez sur motor.ino, dans le menu « Outils » de l'IDE Arduino, définissez « Board=Arduino Nano », « Processor=ATMega328P », définissez le port COM et appuyez sur « Upload » (Ctrl+U).

Lorsque l'outil indique « Upload terminé », l'étape suivante consiste à connecter votre Arduino à Pypilot. Le moyen le plus simple de connecter un Nano à votre Pypilot est d'utiliser le même câble USB que celui utilisé pour sa programmation. Comme ceci :

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/110981993-44db6b00-8368-11eb-8876-92fadeb8c170.png"/></p>

Le Nano connecté en USB sera répertorié comme un périphérique série et Pypilot testera périodiquement toutes les interfaces série. Au bout d'un moment, Pypilot détectera automatiquement la présence de l'Arduino. Lorsque cela se produit, la première chose que vous remarquerez est que les LED Tx et Rx du Nano commenceront à clignoter. Cela signifie que Pypilot communique avec le Nano. Dans le panneau de contrôle du pilote automatique, vous verrez le mot « Arduino » en haut à gauche, le mot « SYNC » au milieu, et parfois « BADVOLTAGE_FAULT ». Ce sont les signes avant-coureurs d'un système performant qui commence à prendre vie. À en juger par le mot « FAULT », nous n'en sommes pas encore là, mais si vous êtes arrivé jusqu'ici, vous méritez vraiment un bon verre.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/110982106-66d4ed80-8368-11eb-999b-38d11beb0e5f.png"/></p>

Cette étape permet simplement de déterminer à quel stade de fonctionnement une fonctionnalité est activée. À ce stade, vous devriez voir ce qui suit :

* Tx et Rx doivent clignoter, ce qui signifie qu'il y a un trafic bidirectionnel entre pypilot et le Nano. Avec une connexion USB, les deux clignotent ou ne clignotent pas. Avec une autre connexion, il se peut que Rx clignote et Tx ne clignote pas. Si c'est le cas, signalez-le.
* Si vous activez le pilote automatique (« AP »), vous devriez voir le mot « Engagé » dans le panneau de commande du pilote automatique, et une autre LED rouge devrait s'allumer sur l'Arduino.
* Des messages d'erreur aléatoires apparaîtront à côté du mot « SYNC », ce qui est normal. Ces messages d'erreur sont appelés « drapeaux » dans Pypilot et sont causés par des broches d'entrée de l'Arduino laissées ouvertes. Laisser des broches d'entrée ouvertes est une mauvaise pratique en électronique ; si elles sont laissées « flottantes », elles sont sensibles aux influences électrostatiques et, pire encore, aux dommages.
***
[Étape 7 : Plugin OpenCPN >>>](Étape-7-OpenCPN-Pypilot-Plugin)