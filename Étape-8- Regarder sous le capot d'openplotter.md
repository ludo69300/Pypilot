* [Services](#services)
* [Scripts de console](#console-scripts)
* [Exécution de Pypilot à l'invite](#running-pypilot-at-the-prompt)
* [Processus](#processes)
* [Réseau](#network)
* [Détermination de la version de Pypilot](#determining-the-pypilot-version)
* [Mise à jour du logiciel Pypilot](#updating-the-pypilot-software)

Comme toujours, c'est agréable de regarder sous le capot, alors jetons un coup d'œil. Cela vous montrera ce que vous devriez voir, afin que vous puissiez le comparer avec vos propres résultats si vous devez diagnostiquer un problème. Cette page suppose quelques connaissances de base sur Linux, mais si vous n'en avez pas, essayez de suivre les exemples et vous serez rapidement opérationnel. Pour obtenir une invite sur OpenPlotter, cliquez sur Raspberry → Accessoires → Terminal. À distance, depuis un PC Windows, téléchargez putty.exe.

> Si vous intégrez des éléments de diagnostic à vos questions sur le forum, vous aurez plus de chances d'obtenir une réponse rapide. Mieux encore : en examinant les fonctionnalités, vous pourrez peut-être répondre à votre propre question avant même de la poser !

## Services
Les logiciels exécutés en arrière-plan sont généralement organisés en « services ». Sur OpenPlotter, les définitions du service Pypilot « systemd » se trouvent dans /etc/systemd/system/ :
```
pi@openplotter:/etc/systemd/system $ ls -1 pypilot*
pypilot_boatimu.service
pypilot.service
pypilot_web.service
```
pypilot.service est le service principal. pypilot.boatimu est le service de boussole, qui peut fonctionner de manière autonome. Pypilot_web est l'interface du navigateur ; elle nécessite que le service Pypilot soit actif.

Pour vérifier si un service est actif, utilisez « systemctl ». Dans cet exemple, le service Pypilot est actif. Ne posez pas de questions à moins d'être certain que votre service Pypilot est en cours d'exécution :
```
pi@openplotter:~ $ systemctl status pypilot
pypilot.service - pypilot
Chargé : chargé (/etc/systemd/system/pypilot.service ; désactivé ; préréglage fournisseur : activé)
Actif : actif (en cours d'exécution) depuis le jeu. 11/03/2021 à 16:50:17 CET ; il y a 3 s
```
## Scripts de console
Les services exécutent des **scripts de console** Python. Il s'agit de scripts shell qui font référence à des scripts Python à l'intérieur de l'« œuf » Python<sup>1</sup>. Les scripts de la console Pypilot se trouvent dans /usr/local/bin/ :
```
pi@openplotter:/usr/local/bin $ ls -1 pypilot*
pypilot
pypilot_boatimu
pypilot_calibration
pypilot_client
pypilot_client_wx
pypilot_control
pypilot_hat
pypilot_scope
pypilot_servo
pypilot_web
```
Normalement, ces scripts sont exécutés par les services, mais vous pouvez les exécuter séparément à des fins de diagnostic. Certains de ces scripts nécessitent l'exécution du service Pypilot principal, d'autres le sont. Certains disposent d'une interface graphique, d'autres d'une interface en ligne de commande. Je ne les explique pas tous, vous trouverez de la documentation dans le fichier readme de GitHub.

> Ce qui est intéressant, c'est que ces scripts Pypilot utilisent par défaut le Pypilot exécuté sur « localhost ». « Localhost » est le terme informatique pour « cet ordinateur ». De nombreux scripts pypilot peuvent être appelés avec l'adresse IP d'un autre pypilot exécuté à distance, par exemple un tinypilot. Ainsi, si vous exécutez un tinypilot, vous pouvez exécuter une portée openplotter à distance sur ses données. J'y reviendrai peut-être plus tard, mais pour l'instant, retenez bien ceci : les composants de l'interface utilisateur d'openplotter ne se limitent pas à un pypilot exécuté sur openplotter. Voir [Étape 12 : Utilisation des outils openplotter à distance](Étape 12-Utilisation-des-outils-openplotter-à-distance)

## Exécution de pypilot à l'invite
Normalement, les services génèrent des journaux dans /var/log, mais sur openplotter, je ne les trouve pas. En cas de problème, vous pouvez désactiver pypilot dans l'interface utilisateur d'openplotter (ou arrêter le service), puis l'exécuter à l'invite de l'utilisateur pi. Vous obtiendrez des informations utiles :
```
pi@openplotter:~ $ pypilot
processus imu 4260
processus nmea 4265
écoute des connexions nmea sur le port 20220
chargement de l'étalonnage des servos /home/pi/.pypilot/servocalibration
AVERTISSEMENT : étalonnage des servos par défaut utilisé !
Pilotes chargés : ['learning', 'basic', 'absolute', 'simple']
avertissement : échec d'ouverture du fichier spécial /dev/watchdog0 en écriture
impossible de caresser le chien de garde
processus gps 4267
processus imu rendu temps réel
[…] etc.
```
N'oubliez pas : si vous rencontrez un problème et que personne d'autre ne peut vous aider, exécutez pypilot (ou tout autre script console) à l'invite. Lisez attentivement le résultat : pypilot vous parle, il est donc préférable de l'écouter. Si vous souhaitez publier le résultat sur le forum, il est préférable de l'enregistrer dans un fichier texte, puis de le joindre à un message. Sauf s'il s'agit d'un court extrait, placez-le entre les balises de code pour que le lecteur sache que vous êtes sérieux et que vous l'aidez à vous aider.

## Processus
Le test le plus rapide consiste à rechercher les **processus** démarrés par les scripts pypilot. Si ce n'est pas le cas, vous avez de quoi réfléchir :
```
pi@openplotter:~ $ ps -ef | grep pypilot
pi 4157 805 0 14:24 00:00:02 /usr/bin/python3 /usr/bin/openplotter-pypilot
pi 4527 1 3 14:27 00:00:36 /usr/bin/python /usr/local/bin/pypilot
pi 4529 1 0 14:27 00:00:00 /usr/bin/python3 /usr/bin/openplotter-pypilot-read
pi 4535 4527 0 14:27 00:00:04 /usr/bin/python /usr/local/bin/pypilot
pi 4536 4527 1 14:27 00:00:18 /usr/bin/python /usr/local/bin/pypilot
pi 4542 4527 0 14:27 00:00:00 /usr/bin/python /usr/local/bin/pypilot
pi 4543 4527 0 14:27 00:00:00 /usr/bin/python /usr/local/bin/pypilot
pi 4546 4527 0 14:27 00:00:00 /usr/bin/python /usr/local/bin/pypilot
pi 4548 4527 0 14:27 00:00:06 /usr/bin/python /usr/local/bin/pypilot
pi 4672 1 0 14:34 00:00:01 /usr/bin/python /usr/local/bin/pypilot_web
pi 4892 1555 0 14:47 00:00:00 grep --color=auto pypilot
```
## Réseau
Un autre aperçu intéressant est celui des **connexions réseau** gérées par pypilot :
```
pi@openplotter:~ $ netstat -antlp|grep python
tcp 0 0 0.0.0.0:23322 0.0.0.0:* LISTEN 4548/python
tcp 0 0 0.0.0.0:20220 0.0.0.0:* LISTEN 4542/python
tcp 0 0 0.0.0.0:8080 0.0.0.0:* LISTEN 4672/python
tcp 0 0 10.10.10.1:23322 10.10.10.1:47460 ESTABLISHED 4548/python
tcp 0 0 127.0.0.1:57390 127.0.0.1:2947 ESTABLISHED 4546/python
```
Vous pouvez voir que pypilot écoute sur 23322, 20220 et 8080. Le premier est la communication interne, utilisée entre autres par les interfaces utilisateur pour communiquer avec pypilot. Le deuxième problème concerne le trafic NMEA entrant/sortant, dont nous avons déjà parlé. Connectez-vous à ce port via Telnet pour voir ce qui se passe. 8080 correspond à l'interface du navigateur.

Vous pouvez également constater que Pypilot utilise le port 2947, qui correspond au démon GPS. Vous pouvez le rechercher sur Google, mais pour faire court : lorsque vous connectez un GPS à une machine Linux, le gpsd le connecte à ce port.

## Déterminer la version de Pypilot
Notez que les versions de Pypilot mentionnées ici sont différentes de celles affichées dans les paramètres d'OpenPlotter (où vous avez installé Pypilot). L'outil OpenPlotter possède son propre système de gestion des versions, indépendant de celui de Pypilot. Il n'existe aucun moyen de déterminer la version de Pypilot dans les interfaces utilisateur. Pour le vérifier, je consulte le script pypilot :
```
pi@openplotter:~ $ which pypilot
/usr/local/bin/pypilot
pi@openplotter:~ $ cat /usr/local/bin/pypilot
#!/usr/bin/python
# EASY-INSTALL-ENTRY-SCRIPT: 'pypilot==0.24','console_scripts','pypilot'
```

La gestion des versions de pypilot n'est pas très stricte. La numérotation des versions donne une indication approximative de la génération du logiciel. Au sein d'un même numéro de version, il peut y avoir de nombreux commits intermédiaires, dont certains peuvent casser des portions de code. Ceci n'est pas un verdict, juste une observation, et il est bon d'en tenir compte lors de la mise à jour du logiciel pypilot.

## Mise à jour du logiciel pypilot
Si vous souhaitez mettre à jour vers la dernière version de développement de pypilot, exécutez les commandes suivantes. Ces commandes peuvent évoluer au fil du temps ; il est donc toujours judicieux de consulter le fichier readme de GitHub. Au moment de la rédaction de cet article (9 avril 2021), la version actuelle est la v0.24. Avant d'effectuer la mise à jour, il est toujours conseillé de désactiver Pypilot. Après la mise à niveau, exécutez Pypilot à l'invite. Si un problème survient, vous le constaterez immédiatement.

```
cd
sudo systemctl stop pypilot pypilot_web pypilot_hat pypilot_boatimu
sudo rm -Rf pypilot/ pypilot_data/
git clone https://github.com/pypilot/pypilot
cd pypilot
sudo python3 setup.py install
```

***
<sup>1</sup> Si le mécanisme de distribution Python vous intéresse, consultez [ici](https://setuptools.readthedocs.io/en/latest/pkg_resources.html); Si vous êtes curieux, jetez un œil aux scripts de la console et reliez les points dans `/usr/local/lib/python3.7/dist-packages/pypilot-0.24-py3.7-linux-armv7l.egg/EGG-INFO/entry_points.txt` _mutatis mutandis_.
***

[Étape 9 : Câblage du Nano >>>](Étape-9-Câblage-du-Nano)