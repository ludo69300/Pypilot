Il existe deux types de connexions de données dans Pypilot :
* Connexions internes
* Connexions externes

## Connexions de données internes
Précédemment, lorsque nous avons évoqué les quatre interfaces utilisateur, nous avons évoqué ce système de tableau noir, utilisé par tous les modules logiciels Pypilot pour la communication interne, et utilisé par les interfaces utilisateur pour interagir avec le système Pypilot. Examinons-le de plus près, puis oublions-le.

Ce système de tableau noir s'appelle en réalité pypilotServer et pypilotClient, si vous regardez sous le capot. Une communication interprocessus est utilisée entre les modules logiciels ; entre les interfaces utilisateur et PypilotServer, il s'agit d'une connexion socket TCP JSON sur le port 23322. Les données stockées dans PypilotServer sont à la fois très volatiles et persistantes : elles contiennent des valeurs de hauteur et de cap, qui changent à 10 Hz, mais aussi des éléments de configuration quasiment fixes. PypilotServer stocke régulièrement les valeurs dans un fichier, ce qui leur permet de survivre aux redémarrages. Pour ceux qui ont déjà utilisé ou étudié Pypilot : ce mécanisme s'appelait auparavant signalk et communiquait via un port différent. Ce changement a eu lieu en 2020, entre Pypilot 1.x et 2.x, ce qui explique pourquoi les anciens clients d'interface utilisateur ne fonctionneront plus avec le nouveau PypilotServer, et inversement.

En conclusion : ce mécanisme sert uniquement à l'échange de données entre les modules logiciels Pypilot en interne et au fonctionnement de l'interface utilisateur. Il s'agit d'un système Pypilot fermé et il n'est pas destiné aux données nautiques.

## Données externes
Les connexions de données externes concernent les données nautiques, comme le cap, l'angle du vent, la vitesse, etc. Pour simplifier, pensez aux données NMEA. Pypilot peut utiliser, ou **consommer**, des données, mais aussi les **produire**. - Données nautiques produites par pypilot
* Cap magnétique
* Tangage
* Gite
* Autre
- Données nautiques consommées par pypilot
* Relèvement au point de cheminement
* Écart de route
* Angle du vent apparent
* Vitesse fond
* Vitesse du vent apparent.

Au minimum, pypilot peut se contenter de ne consommer ou de ne produire aucune donnée. Il peut simplement maintenir le bateau sur un cap précis grâce au cap compas de sa propre IMU.

Les informations de cap magnétique, de tangage et de gite produites par l'IMU peuvent être utiles aux autres utilisateurs du bateau et être mises à disposition.

Les données nautiques consommées par pypilot ont déjà été abordées dans le texte précédent et concernent les modes de pilotage automatique « GPS/Track », « Vent » et « Vent réel ». Si vous fournissez ces données à pypilot, ces modes de pilotage automatique deviennent disponibles et vous pouvez les sélectionner dans l'interface utilisateur.

## Connexions de données externes
Les données externes peuvent être transférées de deux manières :
* Messages NMEA0183 ; et
* Messages SignalK.

## Messages NMEA0183
Les messages NMEA0183 sont gérés par
* le port TCP 20220 ; et
* les interfaces série.

Le **port TCP 20220** **fournit** et **consomme** des messages NMEA. Il suffit d'effectuer une connexion Telnet sur ce port pour comprendre ce que cela signifie :

```
pi@openplotter:~ $ telnet localhost 20220
Connecté à localhost.
$APXDR,A,1.743,D,PTCH*7A
$APXDR,A,0.277,D,ROLL*6B
$APHDM,21.297,M*0C
```

Dans l'extrait ci-dessus, vous voyez comment Pypilot reçoit le tangage, le roulis et le cap magnétique.

Vous pouvez également envoyer des messages NMEA0183 à ce socket 20220. Les messages suivants sont reconnus :
* APB (relèvement par rapport au point de cheminement et erreur de route)
* WMV (angle et vitesse du vent apparent)
* RMC (vitesse fond, latitude/longitude et horodatage GPS)
* RSA (angle de barre)

Il a déjà été mentionné à plusieurs reprises : pour le mode pilote automatique GPS/Track, Pypilot a besoin de messages APB, pour le vent apparent, de messages WMV, et pour le vent réel, de messages WMV et RMC. Pour interfacer ces messages NMEA vers et depuis Pypilot, vous pouvez configurer votre multiplexeur. Plusieurs options s'offrent à vous : un multiplexeur matériel, OpenCPN Data Connections, kplex ou SignalK.

Une option très spéciale est la case à cocher **Transférer NMEA** du plugin OpenCPN Pypilot. Si vous cochez cette case dans le plugin, toutes ces phrases NMEA sont automatiquement synchronisées entre OpenCPN et Pypilot. Aucune question. Plutôt pratique, maintenant vous connaissez le contexte, non ?

Les **interfaces série** consomment également des messages NMEA. Par exemple, vous pouvez brancher un port NMEA0183 vers USB sur votre machine avec une girouette connectée. Le noyau Linux l'enregistrera comme interface série et Pypilot l'analysera, la testera à 4 800 et 38 400 bauds et exploitera ses informations. Ceci s'applique également à l'interface UART native, si elle est disponible. Quant à la **fourniture** de messages NMEA via des interfaces série, cela ne semble pas être le cas.

## Messages SignalK
Jusqu'à récemment, établir ces connexions de données externes avec des messages NMEA via le port TCP 20220 était la seule option. Une option plus moderne est désormais disponible : SignalK. Je vous expliquerai plus en détail SignalK ultérieurement, mais si vous l'avez correctement configuré, Toutes les communications de données nautiques sont gérées automatiquement. Du point de vue de Pypilot, aucune configuration n'est requise. Du point de vue de Signalk, vous devez vous assurer que MDNS est activé dans les paramètres du serveur et approuver la demande d'accès dans le menu Sécurité de Signalk.

***
[Les étapes >>>](Les-étapes)