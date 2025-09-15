Pour activer l'interface, saisissez « sudo systemctl enable pypilot_hat ». Pour que cela fonctionne sans le hat, modifiez le fichier `/etc/systemd/system/pypilot_hat.service` et remplacez « 44444 » par « none ».

Voici le résultat :
```
pi@openplotter:~/pypilot $ pypilot_hat none
GPI pour Raspberry Pi
Chargement du fichier de configuration : /home/pi/.pypilot/hat.conf
Échec du chargement de /proc/device-tree/hat/custom_0 : [Errno 2] Aucun fichier ou répertoire de ce type : '/proc/device-tree/hat/custom_0'
Remplacement de la valeur par défaut du pilote par la ligne de commande « none »
Processus Arduino sur 28138
Renouvellement : échec de définition de la priorité pour 28138 (ID de processus) : Permission refusée
Avertissement : échec de renouvellement du processus Arduino Hat
Aucune configuration Hat, Arduino introuvable
28139 (ID de processus) : ancienne priorité 0, nouvelle priorité 19
(Pause de 30 secondes)
Processus web 28139
Configuration web {'remote': False, 'host': 'pypilot', 'actions': {'auto': [], 'menu': [], 'up': [], 'down': [], 'select': [], 'left': [], 'right': [], 'engage': ['gpio18'], 'disengage': ['gpio23'], '1': [], '-1': [], '2': [], '-2': [], '10': [], '-10': [], 'compassmode': [], 'gpsmode': [], 'windmode': [], 'tackport': [], 'tackstarboard': []}, 'pi.ir': Faux, 'arduino.ir' : Faux, 'arduino.nmea.in' : Faux, 'arduino.nmea.out' : Faux, 'arduino.nmea.baud' : 4800, 'lcd' : {'contrast' : 60, 'invert' : Faux, 'backlight' : 200, 'flip' : Faux, 'language' : 'en', 'bigstep' : 10, 'smallstep' : 1, 'remote' : Faux, 'hue' : 27}}
Processus web sur le port 33333
Client web connecté : c9fb01fcb0ac415a91e56d8432098268
apply ('gpio23', 1) 168451.776421598
apply ('gpio23', 2) 168451.776693334
apply ('gpio23', 0) 168451.776797443
```

Notez que l'interface web sur le port :33333 a un délai de démarrage de 30 secondes.

Si vous n'avez pas de capteur infrarouge, définissez pi.ir sur False dans ~/.pypilot/hat.conf

***
[Étape 6 : Le contrôleur Arduino >>>](Étape-6-Le-contrôleur-Arduino)