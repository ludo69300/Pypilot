Nous avons laissé l'Arduino traîner sur le bureau, toutes les broches d'entrée à la merci des décharges électrostatiques. Il ne reste plus qu'à câbler l'Arduino, c'est-à-dire à construire l'électronique. Tout ce que vous devez savoir se trouve dans les commentaires du code source de motor.ino.

Le **schéma de référence** est disponible sur la boutique Pypilot. Sur la page du contrôleur de moteur Pypilot, faites défiler vers le bas et cliquez sur [schémas](https://pypilot.org/schematics/hbridge_controller.pdf). La première page présente une implémentation du pilote de moteur en pont en H, la seconde explique comment câbler toutes les entrées.

Le schéma de référence est robuste et résistera aux abus. De nombreuses personnes choisissent de ne pas s'en tenir à ce modèle pour diverses raisons :
* Disponibilité problématique de certains composants.
* Disponibilité facile de tous les autres types de contrôleurs de moteur.
* Perspectives différentes.
* Exigences différentes.

Tout d'abord, quelques réflexions sur le modèle.

La séparation de l'Arduino et du contrôleur moteur du Raspberry Pi pypilot s'explique par la séparation des courants PWM élevés de la fragile IMU. Le schéma de référence présente une **connexion série** avec isolation galvanique. Ceci pour plusieurs raisons :
* La connexion est sujette aux interférences et doit être correctement protégée contre les interférences électromagnétiques.
* De plus, le niveau logique 5 V de l'Arduino ne correspond pas au niveau logique 3 V3 du Raspberry.
* Il est préférable d'éviter une boucle de masse, compte tenu des courants moteur élevés, des câbles longs et des signaux PWM haute fréquence.

Les courtes distances et la faible disponibilité des découpleurs galvaniques semblent avoir incité les implémenteurs à se passer de l'isolation galvanique, nécessitant alors l'utilisation de décaleurs de niveau. La connexion USB entre les deux est utilisée dans les prototypes, mais déconseillée pour les configurations opérationnelles.

L'Arduino dispose de trois types de PWM, qui sont en réalité des types de contrôleurs moteur.
• Le type 0 correspond à un pont en H, comme dans le schéma de référence. Quatre broches de sortie de l'Arduino correspondent à 4 FETS dans un pont en H. Le style 0 est sélectionné en reliant la broche D6 de l'Arduino à la masse.
• Le style 1 correspond à un signal de type RC, un signal de 50 Hz sur D9 dont la largeur d'impulsion détermine la position demandée du servo. Les avions RC fonctionnent ainsi. Pour sélectionner le style 1, reliez D6 à VCC. Ce style mentionne également la programmation d'un contrôleur, mais je n'en ai aucune idée. Toute contribution est la bienvenue.
• Le style 2 fournit un signal PWM (D9) et un signal de direction (D2 ou D3). Ce style est un peu le dernier de la gamme, car il n'a pas été très apprécié à ses débuts, jusqu'à récemment. Vous devez l'activer en modifiant le fichier motor.ino et en supprimant le commentaire #define VNH2SP30. L'activation de ce style a déjà perturbé les relevés des capteurs ; assurez-vous donc d'avoir la dernière version du fichier motor.ino.

J'ai déjà analysé les signaux émis ; voir cette vidéo : https://youtu.be/Z4K_Hwsje40

Les capteurs liés aux broches Arduino incluent la température du contrôleur, la température du moteur, le courant du moteur, la tension du pilote, le retour d'angle de barre et les interrupteurs de fin de course. Certaines mesures de ces capteurs sont traitées par l'Arduino (fin de course, courant, tension), tandis que d'autres sont renvoyées au logiciel pypilot et utilisées pour le fonctionnement du servo et la collecte de statistiques. Si vous n'implémentez pas de capteur, reliez l'entrée à la masse et désactivez le capteur dans le code motor.ino. Il est préférable d'implémenter tous les capteurs, car la plupart sont là pour la sécurité.

Dans les algorithmes de pilotage « de base » et « simple », le retour d'angle de barre n'est pas utilisé pour le positionnement du servo, mais uniquement pour les arrêts progressifs de fin de course. Dans l'algorithme de pilotage « absolu » récemment publié, le retour d'angle de barre est utilisé pour le positionnement.

Pour l'instant, ce petit exercice de réflexion sur l'électronique est terminé. Si vous n'êtes pas un passionné d'électronique, il est préférable d'acheter un contrôleur de moteur Pypilot prêt à l'emploi.

***
[Étape 10 : Installation de Tinypilot >>>](Étape-10-Installation de Tinypilot)