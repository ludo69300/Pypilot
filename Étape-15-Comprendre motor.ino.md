Motor.ino peut être un logiciel complexe. Je n'en ai pas non plus les connaissances classiques. Voici ce que je sais.

Le mécanisme utilisé est un « PWM piloté par interruption », et si vous voulez vraiment comprendre le code, commencez par chercher sur Google. Il s'agit de temporisateurs matériels intégrés qui forcent le processeur à interrompre son activité et à accéder au code défini comme « gestionnaire d'interruptions ». Vous pouvez passer toute votre carrière à développer des logiciels sans avoir à le rencontrer.

La sortie générée sur les broches de données Arduino est assez générique et devrait prendre en charge une large gamme de pilotes de moteur. La fin de cette page répertorie des recettes éprouvées basées sur le moteur.ino standard. Plutôt que d'essayer de modifier motor.ino, il est conseillé aux développeurs de Pypilot d'utiliser un pilote éprouvé compatible avec le moteur.ino standard.

## Modes de fonctionnement de Motor.ino
Il existe trois modes de fonctionnement pour chaque type de sortie : Dans le code, ils sont appelés « pwm_style » :
* pwm_style == 0 « Pont en H », sélectionné en abaissant D6
* pwm_style == 1 « Style RC », sélectionné en abaissant D6
* pwm_style == 2 « Style PWM », sélectionné en décommentant la ligne « //#define VNH2SP30 »

**Pont en H** : ce mode de fonctionnement permet aux quatre sorties de piloter « directement » les grilles des quatre transistors d'un pont en H. D2 et D3 sont les bornes basses, D9 et D10 les bornes hautes. J'ai mis « directement » entre parenthèses, car il faut toujours de l'électronique entre l'Arduino et le transistor. Il arrive souvent que, dans un pont en H, le côté haut ou bas soit toujours ouvert pendant le fonctionnement, tandis que l'autre côté (opposé) gère la largeur d'impulsion variable (PWM). Pendant la phase d'arrêt du signal PWM, l'énergie du moteur circule dans le transistor de l'autre côté.

Voici à quoi ressemble le moteur lorsqu'il tourne un peu dans un sens puis s'arrête. On observe une phase de montée, une phase stationnaire et une phase de descente. Pendant les phases de montée et de descente, la fréquence PWM est de 15 kHz, et pendant la phase stationnaire, de 65 Hz. Cette dernière fréquence permet de minimiser la dissipation de puissance tout en chargeant les condensateurs nécessaires au pilotage des grilles des MOSFET côté haut. Ce n'est pas grave si vous ne comprenez pas encore, regardez simplement l'image :

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/114552192-9c6d3f00-9c64-11eb-9eca-b6a267bad01b.png"></p>

Il y a quelque chose d'étrange avec le côté opposé bas : il affiche également un signal PWM. Je n'en ai aucune idée, alors si quelqu'un le sait, n'hésitez pas à laisser un commentaire.

Si vous souhaitez construire votre propre pont en H et que vous n'avez pas suivi de formation en électronique, préparez-vous à endommager certains composants. Des schémas de référence sont disponibles sur le site pypilot.org, et ils sont assez robustes. Si vous pensez être plus compétent, mieux vaut vous en assurer.

Si vous souhaitez utiliser un pilote de pont en H disponible dans le commerce, le mode pwm_style == 0 est souvent le plus approprié. D9 et D10 sont les signaux PWM pour la gauche et la droite, et D2 et D3 sont les signaux d'activation pour la gauche et la droite. Le signal opposé, côté haut, est un peu étrange ici, et je ne l'ai pas encore testé personnellement.

**PWM-style** - Ce mode est sélectionné lorsque vous décommentez une ligne dans motor.ino (voir ci-dessus). La sortie présente un niveau d'abstraction légèrement supérieur : nous avons PWM, activation et direction (deux fois). Donc, si votre pilote possède ce type d'entrées, c'est le mode de fonctionnement à sélectionner.

Ce mode de fonctionnement a été quelque peu négligé par le passé en raison de la disponibilité de pilotes fiables de ce type. Depuis 2020, le mode PWM est quasiment acceptable dès le départ.

L'inconvénient de ce mode de fonctionnement est qu'il ne fonctionne qu'à une fréquence PWM (audible) de 1 000 Hz. Cela provoque un sifflement du moteur. Je pensais que cela m'irriterait, mais j'ai fini par l'apprécier.

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/114552387-d6d6dc00-9c64-11eb-9894-145df3162db0.png"></p>

**Style RC** - Ce style fonctionne apparemment avec des contrôleurs ESC (contrôleur de vitesse électronique). Il semblerait que de nombreuses personnes s'y connaissent, mais je n'en fais pas partie, et je ne sais pas quoi chercher sur Google pour éviter d'être submergé par les résultats d'Aliexpress et autres. Il ne fournit qu'un seul signal, un signal de 50 Hz avec une largeur d'impulsion variable. Apparemment, cela suffit pour piloter un moteur à gauche et à droite. Je n'ai pas encore trouvé de première personne à l'avoir utilisé. Si c'est votre cas, n'hésitez pas à vous faire connaître.

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/114552503-f5d56e00-9c64-11eb-84e6-5cc74f356967.png"></p>

## Combinaisons éprouvées
À moins que vous ne soyez un véritable passionné d'électronique, je vous conseille d'en acheter un prêt à l'emploi. Cette liste est censée présenter uniquement des pilotes commerciaux prêts à l'emploi compatibles avec motor.ino.

Quoi | Qui | Détails
-- | -- | --
IBT-2 | [damien](https://forum.openmarine.net/showthread.php?tid=3388&pid=19024#pid19024) | pwm_style=2, D9 vers R_EN et L_EN, D2, D3 vers RPWM ou LPWM
Pololu n° 2991 : | ironman | pwm-style=2, D9 vers PWM, D2 vers DIR, D10 vers <SPAN STYLE="text-decoration:overline;">SLP</SPAN>
LN298N | [kinefou](https://forum.openmarine.net/showthread.php?tid=2405&pid=12993#pid12993)
VNH5019 | CapnKernel

***

Voir aussi : [Démystifier motor.ino](https://youtu.be/Z4K_Hwsje40) _Mise en ligne YouTube_