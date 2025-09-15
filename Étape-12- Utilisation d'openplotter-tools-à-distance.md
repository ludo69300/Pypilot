Comme indiqué précédemment, les outils d'openplotter se connectent par défaut au pypilot sur l'hôte local. Ils peuvent cependant être utilisés sur un pypilot distant. Ceci est utile pour se connecter à un tinypilot connecté.

Ceci s'applique aux scripts suivants :
* pypilot_client [-s adresse IP]
* pypilot_client_wx [adresse IP]
* pypilot_calibration [adresse IP]
* pypilot_control [adresse IP]
* pypilot_scope [adresse IP]

Après une connexion réussie à un pypilot distant, l'adresse IP est stockée dans le fichier ~/.pypilot/pypilot_client.conf. Toute invocation ultérieure de l'un des scripts ci-dessus utilisera par défaut cette adresse IP.

Pour un accès distant, pypilot_client doit être invoqué avec l'option -s <adresse IP>. Pour les autres scripts, l'option -s n'est pas nécessaire.

Exemple :

```
# Dans cet exemple, 10.10.10.2 est l'adresse sur le tinypilot
pi@openplotter:~ $ pypilot_client -s 10.10.10.2
[base de données entière vidée]
pi@openplotter:~ $ pypilot_client -s 10.10.10.2 -c ap.heading_error
ap.heading_error = -2.122
ap.heading_error = -2.154
ap.heading_error = -2.158
^C
pi@openplotter:~ $ pypilot_calibration
[...]
```
Ceci s'applique non seulement lorsque vous appelez ces scripts depuis la ligne de commande, mais aussi lorsque vous les appelez depuis l'interface openplotter : à partir de ce moment, votre pypilot est le pypilot externe. À mon avis, c'est un bon exemple de la qualité de l'implémentation d'une architecture modulaire.
***
[Étape 13 : Connexions SignalK >>>](Étape-13-Connexions SignalK)