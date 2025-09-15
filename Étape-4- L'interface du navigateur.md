Sur l'écran de l'interface utilisateur d'openplotter pypilot, vous trouverez deux boutons supplémentaires : « Contrôle du navigateur » et « Ouvrir ». « Contrôle du navigateur » est un bouton à bascule que vous pouvez activer et qui active l'interface du navigateur.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/110981380-5a9c6080-8367-11eb-8cf5-785f5263cb42.png"/></p>

Lorsque vous cliquez sur « Ouvrir », un navigateur démarre sur http://localhost:8080, où se trouve l'interface du navigateur.

> Remarque : sur Tinypilot, l'interface se trouve sur le port :80.

Vous pouvez également ouvrir l'interface du navigateur depuis la machine sur laquelle vous exécutez votre client VNC. Tapez simplement http://10.10.10.1:8080/ et l'interface du navigateur s'affiche.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/110981510-8881a500-8367-11eb-8679-792757488941.png"/></p>

Au moment de la rédaction de cet article, le message « Serveur pypilot : Non connecté » s'affiche. Cela semble être lié à une incompatibilité de bibliothèque, qui peut être résolue avec la solution suivante :
```
sudo pip3 uninstall python-socketio python-engineio flask-socketio
sudo pip3 install "flask-socketio<5" "python-socketio<5"
```

> Remarque : si vous exécutez pypilot_web [à l'invite](https://github.com/pypilotWorkbook/workbook/wiki/Step-8-Looking-under-the-hood-of-openplotter#running-pypilot-at-the-prompt), à des fins de débogage, il est utilisé sur le port 8000 au lieu de 8080. Vous devez donc saisir http://10.10.10.1:8000.

***
[Étape 5 : L'interface HAT >>>](Étape-5-L'interface HAT)