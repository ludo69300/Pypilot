Examinons quelques éléments de l'interface utilisateur pertinents pour l'instant. Cliquez sur le bouton « Contrôle ». L'écran **Contrôle du pilote automatique** apparaît. Voici ce qui s'affiche :

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/110976103-c7f8c300-8360-11eb-9905-306d79e21ec2.png"/></p>

* L'écran **Contrôle du pilote automatique** est l'équivalent du panneau de boutons physiques des pilotes automatiques de bateaux. Vous y verrez un cap compas. Tournez votre Raspberry et vous le verrez bouger. Le gros bouton rouge AP active le pilote automatique. Pour l'instant, seul le mode Compas est activé. Nous ne lui transmettons pas encore de données de trajectoire ni de vent, ce qui est logique.
* Le contrôle du pilote automatique et l'interface utilisateur de pypilot disposent tous deux d'un bouton **Calibration**, qui pointe vers la même fonction. Cette fonction comporte cinq onglets, dont le premier est Alignement. Vous y trouverez le bouton très important « Bateau à niveau ». Cliquez dessus pour que la représentation du bateau se redresse. Je n'ai jamais pu utiliser le reste de la calibration à la maison. Cela ne semble fonctionner que sur le bateau. Ne me demandez pas pourquoi.
* Le bouton **Client** vous amène directement à la base de données Blackboard de pypilotServer. Vous pouvez voir l'intégralité de cette base de données et de nombreux éléments modifiables. C'est extrêmement dangereux, car lorsque vous faites défiler la liste, le bouton de la souris touche parfois un curseur et vous modifiez instantanément les valeurs. Il n'y a pas de bouton Annuler. Je respecte cet écran avec le plus grand respect : nous n'avons pas encore effectué de sauvegarde de la base de données (des paramètres).
* La fonction **Scope** est beaucoup plus facile à utiliser, mais elle n'est pas intéressante à la maison, seulement sur le bateau. L'erreur ap.heading_error est intéressante si vous ajustez vos gains. Mais nous n'en sommes pas encore là !
* Pour en revenir au contrôle du pilote automatique, quelques points méritent d'être expliqués.
* Le « none » en haut à gauche signifie qu'aucun contrôleur Arduino n'a été trouvé.
* « Disengaged » signifie que l'actionneur (moteur) n'actionne pas le gouvernail actuellement.
* Le menu déroulant « Pilot » affiche d'autres algorithmes de pilotage. La valeur par défaut est Basic, mais vous pouvez également choisir Simple. L'apprentissage est un algorithme auto-apprenant, mais il est expérimental.
* Le curseur horizontal dirigerait directement le gouvernail.
* Tous les curseurs verticaux avec des chiffres et des lettres représentent les gains individuels de l'algorithme de pilotage choisi. La coloration est un retour d'information sur les composants individuels de l'algorithme de pilotage qui font de leur mieux pour diriger le bateau. Si vous tournez l'IMU, vous constaterez une certaine activité.

Nous avons pris nos marques sur la première interface utilisateur de Pypilot. Il s'agissait de l'interface OpenPlotter. Presque toutes les fonctionnalités présentées ici sont également disponibles dans d'autres interfaces utilisateur.

***
[Étape 4 : L'interface du navigateur >>>](Étape-4- L'interface du navigateur)