Tout d'abord, SignalK est un vaste sujet. C'est un excellent logiciel récemment intégré à OpenPlotter, OpenCPN et Pypilot, qui permet l'échange de données nautiques entre appareils et composants logiciels. On peut l'appeler multiplexeur, mais c'est bien plus que cela. Il remplace cependant Kplex. Si vous souhaitez établir une connexion avec Pypilot, il est préférable de vous renseigner d'abord sur SignalK.

Il existe trois façons d'importer les données des capteurs Pypilot dans SignalK : **nmea**, **script d'arrière-plan OpenPlotter** et **signalk zeroconf**.

## NMEA
L'ancienne méthode consistait à demander à SignalK de les extraire du port NMEA 20220/TCP de Pypilot. L'outil Pypilot d'OpenPlotter dispose de cette connexion prête à l'emploi ; si vous ajoutez la connexion dans cet outil, une connexion client NMEA est ajoutée à SignalK, qui extrait ces données de Pypilot. Cette connexion NMEA/TCP semble n'envoyer que **navigation.headingMagnetic** dans le navigateur de données de SignalK. Bien que le tangage et le roulis soient disponibles dans les messages NMEA, SignalK ne semble pas pouvoir les traiter car ils sont encapsulés dans des phrases XDR. Je pense. Bien qu'OpenPlotter dispose de cette fonctionnalité pour établir cette connexion dans SignalK, vous pouvez tout aussi bien la configurer manuellement dans SignalK.

## Script d'arrière-plan OpenPlotter
Le **script d'arrière-plan OpenPlotter** est un processus d'arrière-plan OpenPlotter<sup>1</sup> qui extrait les données de tangage et de roulis de Pypilot et les transfère dans SignalK. Le script est toujours exécuté avec Pypilot, mais les données ne sont visibles que s'il existe une connexion SignalK en écoute sur 20220/UDP, et c'est ce que l'autre « connexion » crée. Cela semble être une solution de contournement pour intégrer les données de tangage et de gîte dans SignalK, car la méthode NMEA ne fonctionne pas.

<p align="center"><img width="85%" src="https://user-images.githubusercontent.com/17980560/111814295-980a6c00-88da-11eb-8117-e65e2c1cde07.png"></p>

Le script openplotter a une fonction différente lorsque vous activez le mode « compas seul » de Pypilot. Dans ce mode, seul le processus boatimu est exécuté, ce qui signifie qu'aucun processus Pypilot ne gère le trafic NMEA sur 20220/TCP. Dans ce cas, le script openplotter ajoute navigation.headingMagnetic, ce qui lui permet de prendre tout son sens.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/111814758-25e65700-88db-11eb-8168-cc852f00bc64.png"></p>

Les données provenant du script d'arrière-plan d'OpenPlotter s'identifient à la source « OpenPlotter.I2C.pypilot ». Si vous la voyez dans le navigateur de données Signalk, vous savez d'où elle provient :

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/111815225-b6bd3280-88db-11eb-9e79-d048f1441863.png"></p>

## Signalk zeroconf
La méthode **moderne** pour connecter Pypilot à Signalk consiste à connecter Pypilot à Signalk. Cette méthode, introduite en mars 2020 (v0.21), nécessite une version récente de Pypilot ; les tests ont été réalisés avec la v0.24. D'un point de vue technique, cette méthode est excellente : il n'y a pas d'envois incessants de messages NMEA vers le monde entier ; il n'y a qu'un seul stockage opérationnel de données nautiques, et en tant que client, vous vous abonnez aux mises à jour des données dont vous avez besoin.

Cette connexion Signalk fonctionne sans configuration : Pypilot trouve le serveur Signalk tout seul en émettant une « requête de découverte de service » sur le réseau. Un agent (nous y reviendrons) répond à Pypilot avec l'adresse de Signalk, puis Pypilot se connecte à Signalk à cette adresse. Pypilot émet ensuite une demande d'accès à Signalk, et lorsque vous approuvez cette demande manuellement dans Signalk, une connexion permanente est établie.

Cela nécessite la configuration de certains éléments, qui ne sont pas présents par défaut.

Sur openplotter, vous devez installer quelques paquets :
```
sudo pip3 install zeroconf
sudo apt install python3-zeroconf python3-websocket
```
Comment le savez-vous ? Si vous exécutez pypilot à l'invite et lisez le résultat ligne par ligne, il est présent. C'est la seule façon de résoudre ce type de problème.

Au démarrage de pypilot, vous constaterez qu'il est capable d'envoyer une requête de service pour signalk :
```
signalk zeroconf service add myrouter._http._tcp.local. _http._tcp.local.
```
Si personne ne répond à cette requête de service, rien ne se passe. L'agent qui devrait répondre est un **serveur mdns** : « découverte de service DNS multicast ». Ce service n'est normalement pas disponible sur votre réseau, mais il est fourni par SignalK si vous avez coché Serveur->Paramètres->mdns. N'oubliez pas que vous devez redémarrer le serveur SignalK pour cela.

<p align="center"><img width="60%" src="https://user-images.githubusercontent.com/17980560/111764644-b5701380-88a3-11eb-96df-c2c02ef447ad.png"></p>

Avec mdns activé, votre serveur Pypilot trouvera SignalK :
```
serveur SignalK trouvé 10.10.10.1:3000
```
Lorsque pypilot se connecte à signalk (via le service Web à 3000/signalk), il se trouve non autorisé et publie donc une demande d'accès (requestId). Sonde de signal... 10.10.10.1:3000
Signalk détecté : ws://10.10.10.1:3000/signalk/v1/stream?subscribe=none
Signalk n'a pas réussi à se connecter. État de la poignée de main : 401 Non autorisé.
Signalk post {'state': 'PENDING', 'requestId': '384b3471-9e76-401c-a663-c84f2fc30cad', 'statusCode': 202, 'href': '/signalk/v1/requests/384b3471-9e76-401c-a663-c84f2fc30cad', 'ip': '::ffff:10.10.10.1'}
URL d'accès à la requête de signalk http://10.10.10.1:3000/signalk/v1/requests/384b3471-9e76-401c-a663-c84f2fc30cad
signalk vérifie si le jeton est prêt http://10.10.10.1:3000/signalk/v1/requests/384b3471-9e76-401c-a663-c84f2fc30cad {'state': 'PENDING', 'requestId': '384b3471-9e76-401c-a663-c84f2fc30cad', 'statusCode': 202, 'href': '/signalk/v1/requests/384b3471-9e76-401c-a663-c84f2fc30cad', 'ip': '::ffff:10.10.10.1'}
```
Dans signalK, cette demande d'accès apparaît dans le menu Sécurité. Lors de l'approbation, définissez les autorisations sur Lecture/Écriture, sinon Pypilot ne pourra toujours pas écrire ses mesures dans signalk. Après une interrogation périodique (« vérifier si le jeton est prêt »), pypilot reçoit un jeton de sécurité et peut établir la connexion :
```
signalk received token eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJkZXZpY2UiOiJweXBpbG90LTg4NzQ2NDg3OTQ4IiwiaWF0IjoxNjE2MTQ2NTMzLCJleHAiOjE2NDc3MDQxMzN9.KSADIUWyEYKB9Go4_kltxidwb8fIj_u9EwI6mlyh_RI
signalk connected to ws://10.10.10.1:3000/signalk/v1/stream?subscribe=none
```
À partir de ce moment, vous trouverez également navigation.attitude dans votre navigateur de données signalk.

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/111764181-3b3f8f00-88a3-11eb-9848-ae39297a0768.png"></p>

***

Cette connexion signalk initiée par Pypilot devrait pouvoir remplacer tous les échanges de données nautiques avec Pypilot : vous ne devriez plus avoir besoin d'envoyer des données NMEA de vent, de trace et de GPS à Pypilot. Lorsque cette connexion fonctionnera, le port 20220 sera obsolète.

En résumé, pour faire fonctionner Signalk Zeroconf :
* installer les paquets
* activer mdns
* approuver la demande d'accès, lecture-écriture

***
> <sup>1</sup> À des fins d'historique et d'exhaustivité, le script d'arrière-plan d'OpenPlotter se trouve dans `/usr/lib/python3/dist-packages/openplotterPypilot/openplotterPypilotRead.py` et s'exécute comme un service systemd nommé `openplotter-pypilot-read`. La version actuelle est incompatible avec la dernière version de Pypilot, mais cette dernière inclut le client Signalk Zeroconf.

***
[Étape 14 : Le contrôleur de moteur Pypilot >>> ](Étape-14-Le-contrôleur-de-moteur-Pypilot)

* Une erreur persiste dans le journal ; je n'ai pas encore déterminé son origine, mais cela ne semble pas avoir d'importance.
* Ceci pourrait être lié au problème connu : Pypilot se connecte à tout autre périphérique annoncé sur mdns. https://github.com/pypilot/pypilot/issues/89
```
Exception dans le thread zeroconf-ServiceBrowser__http._tcp.local._3028284512 :
Traceback (dernier appel le plus récent) :
Fichier « /usr/lib/python3.7/threading.py », ligne 917, dans _bootstrap_inner
self.run()
Fichier « /home/pi/.local/lib/python3.7/site-packages/zeroconf/__init__.py », ligne 1759, dans run
state_change=service_type_state_change[1],
Fichier « /home/pi/.local/lib/python3.7/site-packages/zeroconf/__init__.py », ligne 1513, dans fire
h(**kwargs)
Fichier « /home/pi/.local/lib/python3.7/site-packages/zeroconf/__init__.py », ligne 1611, dans on_change
listener.add_service(*args)
Fichier « /usr/local/lib/python3.7/dist-packages/pypilot-0.24-py3.7-linux-armv7l.egg/pypilot/signalk.py », ligne 123, dans add_service
si properties['swname'] == 'signalk-server' :
KeyError : 'swname'
```