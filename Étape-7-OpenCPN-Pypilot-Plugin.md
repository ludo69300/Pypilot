C'est mon interface préférée. Elle est élégante et intuitive. Je suis un fervent utilisateur d'OpenCPN et j'utilise plusieurs plugins. Pour installer le plugin pypilot :

* Démarrez Raspberry → Openplotter → OpenCPN
* Dans la barre d'outils, cliquez sur la première icône en forme de roue dentée. Pour référence ultérieure, il s'agit du menu « Options ».
* Cliquez sur l'onglet « Plugins ».
* En bas à gauche, cliquez sur « Mettre à jour le catalogue de plugins : Master ». Cela prend quelques secondes ; assurez-vous que votre câble Ethernet est branché.
* Faites défiler jusqu'à pypilot et cliquez dessus ; cliquez sur Installer, puis cochez la case Activer et cliquez sur OK.
* La barre d'outils affiche maintenant une nouvelle icône en forme de voilier. Il s'agit du plugin pypilot. Cliquez dessus.
* Cliquez sur le deuxième bouton en bas. Il indique « Configuration », mais vous ne le verrez pas car la fenêtre est trop petite et vous ne pouvez pas l'agrandir manuellement. Cliquez dessus quand même, sélectionnez 127.0.0.1 pour « hôte », puis cliquez sur OK.
* À ce stade, le plugin devrait se connecter et s'activer. Le bouton pypilot passera du gris au rouge, puis au vert si vous l'activez. Il existe quelques différences mineures avec l'interface utilisateur d'openplotter :
* Si l'Arduino n'est pas présent, l'écran affiche « pas de contrôleur de moteur » au lieu de « Aucun ». S'il est présent, l'écran affiche « inactif » ou « OK » au lieu de « Arduino ».
* Les gains sont désormais sous un bouton, il n'y a plus d'oscilloscope, mais un écran de statistiques. * Au lieu d'un curseur de gouvernail, vous disposez désormais des boutons gauche et droit habituels :

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/110982344-b5828780-8368-11eb-9178-347daf87390f.png"/></p>

<a href="#oldversion">&nbsp;</a>
* Au moment de la rédaction de cet article, cela n'a pas fonctionné immédiatement : le plugin affiche « déconnecté », bien que Pypilot soit actif et que l'adresse de l'hôte soit correcte. Une vérification rapide révèle la cause : la version du logiciel Pypilot installée par OpenPlotter est ancienne (v0.16) et écoute toujours sur l'ancien port interne signalk/21311, au lieu du nouveau pypilotServer/23322 ; port auquel le tout nouveau plugin OpenCPN tente de se connecter. * Si tel est le cas, [mettre à niveau pypilot](Étape 8 - Regarder sous le capot d'openplotter#mettre à niveau pypilot). Ceci est décrit à l'étape suivante.
***
[Étape 8 : Regarder sous le capot >>>](Étape-8- Regarder sous le capot d'openplotter)