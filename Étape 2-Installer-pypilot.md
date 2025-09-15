Dans cette étape, vous allez installer Pypilot. Avec Openplotter, c'est très simple. Comme indiqué précédemment, cela installera le composant logiciel Pypilot et l'interface utilisateur d'Openplotter.

* L'icône Raspberry apparaît en haut à gauche. Il s'agit du menu Démarrer. Quand je dis Raspberry, je parle de cette icône.
* Cliquez sur « Paramètres Raspberry » Openplotter. Dans le premier onglet, vous verrez la liste des applications Openplotter. L'option « Actualiser » s'affichera au moins sept fois à l'écran ; ne l'ignorez pas. Il s'agit de ce bouton :

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/110821208-25710f00-8290-11eb-80f0-70bf8b9ef610.png"/></p>

* À ce stade, le panneau Paramètres se bloque. Il se bloque car il tente d'accéder à Internet. Vous pouvez maintenant vous comporter comme un professionnel de l'informatique avisé et essayer une solution qui ne vous oblige pas à vous lever de votre chaise, ou vous procurer un bon vieux câble Ethernet et le brancher sur le Raspberry Pi. Pour plus de rapidité, je vous conseille la deuxième option. L'écran s'actualisera avec les numéros de version.
* Faites défiler jusqu'à Pypilot, cliquez dessus, installez-le et confirmez. Cela prendra quelques minutes. Vous aurez largement le temps de vous préparer une tasse de thé.
Une fois terminé, vous verrez un nouvel élément de menu : Raspberry – Openplotter – Pypilot. Cliquez dessus. Votre première interface utilisateur Pypilot, celle d'Openplotter, s'affiche.

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/110822041-f6a76880-8290-11eb-9a82-f24b67e2c4a7.png"/>
<br><i>Anormal</i></p>

* Trois problèmes se produisent. Premièrement, l'IMU n'a pas pu être détectée. Je vous l'ai dit : Pypilot a besoin d'une IMU. Celle-ci doit être connectée à l'embase GPIO du Raspberry avec quatre fils. Si vous êtes malin, vous en avez commandé un à l'avance sur la boutique Pypilot et vous l'avez simplement collé à l'endroit sur le connecteur GPIO. Si vous pensez avoir été malin et avez commandé un clone sur une boutique en ligne douteuse, vous devez souder les fils pour connecter 3v3, SCL, SDA et GND, et croisez les doigts.

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/110822270-31110580-8291-11eb-8fc6-72dd6e42b019.png"/></p>

* La ligne en bas de l'écran indique que vous devez activer l'i2c. Accédez à Raspberry Préférences - Configuration du Raspberry Pi, cliquez sur l'onglet Interfaces et activez l'i2c. Revenez à l'interface utilisateur de Pypilot et cliquez sur Actualiser. L'IMU sera désormais visible.
* Enfin, passez de « Désactivé » à « Pilote automatique ». Une fois les modifications appliquées, votre pypilot sera opérationnel ! Ne laissez pas votre thé refroidir.

<p align="center"><img width="80%" src="https://user-images.githubusercontent.com/17980560/112214726-d2983f80-8c1f-11eb-8a0f-6716891fa542.png"/>
<br><i>bien</i></p>

***

[Étape 3 : L'interface utilisateur d'openplotter >>>](Étape-3-L'interface-utilisateur-d'openplotter)