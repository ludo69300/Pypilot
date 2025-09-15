La première étape consiste à installer Openplotter, car c'est de loin le moyen le plus simple et le plus rapide de faire fonctionner Pypilot.

Pour installer Openplotter, suivez les instructions de [ce lien](https://openplotter.readthedocs.io/en/latest/getting_started/installing.html) : Installation / Basique. Vous n'avez besoin que d'une carte SD, d'un Raspberry Pi et d'un lecteur de carte SD. « Headless » signifie que vous n'avez pas besoin de connecter d'écran, de souris ou de clavier à votre Raspberry : vous y accédez via VNC, un système de bureau à distance.
* **Téléchargez** « Openplotter Headless » depuis [ce lien](https://openplotter.readthedocs.io/en/latest/getting_started/downloading.html) et décompressez-le. J'ai obtenu un fichier image nommé 2020-12-16-OpenPlotter-v2-Headless.img après décompression. * **Gravez** ce fichier sur la carte SD avec [Raspberry Pi Imager](https://www.raspberrypi.org/software/). Dans la liste déroulante « Système d'exploitation », faites défiler jusqu'en bas, puis sélectionnez « Utiliser personnalisé », puis le fichier .img.
* Une fois la carte SD gravée, retirez-la du lecteur, **insérez-la** dans le Raspberry et branchez le cordon d'alimentation sur le Raspberry.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/110975789-7819fc00-8360-11eb-8c05-6d49a67040b3.png"/></p>

* Après 2 minutes maximum, un point d'accès Wi-Fi appelé « openplotter » apparaîtra. Connectez-y votre ordinateur. Le mot de passe Wi-Fi est 12345678.
* Windows a la fâcheuse habitude d'interrompre les connexions Wi-Fi lorsqu'il ne parvient pas à pinguer Bill Gates. Surveillez donc votre connexion ; vous devrez peut-être vous reconnecter plusieurs fois.
* Si elle persiste, vous obtiendrez une adresse IP dans la plage 10.10.10.0 et pourrez pinguer 10.10.10.1. Il s'agit de votre openplotter raspberry.
* Téléchargez maintenant RealVNC VNC Viewer depuis [ici](https://www.realvnc.com/en/connect/download/viewer/windows/). Ouvrez-le et connectez-vous à 10.10.10.1:5900. Le nom d'utilisateur est pi, le mot de passe est raspberry. Vous verrez le bureau et un avertissement s'affichera. Si vous êtes sous Linux, vous pouvez utiliser Remmina (Ubuntu) ou un autre client VNC.
* Acceptez l'avertissement et cliquez plusieurs fois sur Suivant. En cours de route, vous devrez attribuer un nouveau mot de passe à l'utilisateur pi. Voici ce qui s'affiche :

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/110821795-b516bd80-8290-11eb-9b91-c281b9c4ad36.png"/></p>

* N'oubliez pas de vous faire plaisir ! Vous avez terminé la première étape.

***
[Étape 2 : Installer pypilot >>>](Étape 2-Installer-pypilot)