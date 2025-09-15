Pypilot est soit en mode veille, soit en mode marche. S'il est activé, quatre modes de pilotage automatique sont disponibles : Compas, GPS, Vent et Vent réel. Voici une liste :
* Veille
* Compas
* GPS
* Vent
* Vent réel

Voici chacune de ces fonctions de base.

## Mode veille
Dans ce mode, chaque interface utilisateur affiche le **cap compas actuel**. Il s'agit du cap compas fourni par l'IMU de Pypilot. Si vous possédez un autre compas électronique sur votre bateau, Pypilot l'ignorera totalement.

Outre l'affichage du cap compas, des boutons permettent de **déplacer le gouvernail** vers la gauche ou la droite, voire de le centrer. Cela permet d'aligner l'actionneur avec la barre afin de pouvoir l'accrocher. Et si vous êtes paresseux, vous pouvez diriger le bateau avec les boutons.

Ensuite, vous pouvez choisir parmi une liste de **modes de pilotage automatique**. Le mode compas est par défaut et toujours disponible. Les options GPS, Vent et Vent réel ne sont disponibles que sous certaines conditions, que j'expliquerai ci-dessous. Une fois le mode de pilotage automatique sélectionné, appuyez sur le bouton AP (pilote automatique) pour accéder au mode souhaité.

## Mode Compas
En mode Compas, le bateau suit le cap commandé. Lorsque vous entrez en mode Compas, le cap commandé est par défaut le cap actuel, mais vous pouvez le modifier à l'aide des boutons <<, <, >, >>. J'explique ici de manière complexe ce que tout le monde sait faire avec un pilote automatique, mais j'introduis un peu de terminologie, alors soyez indulgents.

C'est aussi simple que cela, mais à ce stade, deux points sont importants à mentionner.

Tout d'abord, le **cap compas** de Pypilot. Comme indiqué précédemment, il s'agit du cap compas déterminé par Pypilot à partir des relevés de l'IMU. Un cap compas est une valeur unidimensionnelle, alors pourquoi aurait-on besoin d'une IMU 9 axes pour le déterminer ? En bref, chaque bateau déforme le champ magnétique, non seulement différemment selon le cap, mais aussi selon le roulis et le tangage. Tout navigateur connaît les tables d'écart, mais pour piloter un pilote automatique, il est également nécessaire de connaître l'écart pour différentes positions de roulis et de tangage. Pypilot détermine en permanence ces valeurs d'écart en fonction de l'IMU, grâce à un processus appelé autocalibrage. Cependant, un étalonnage initial de base est nécessaire. Il en résulte une référence de compas très stable pour la navigation.

Deuxième point à mentionner : les **gains** du mécanisme de direction. Ils déterminent la réactivité de la direction et la suppression des dépassements. De nombreux gains sont disponibles. Leurs valeurs varient d'un bateau à l'autre et doivent donc être correctement réglés.

Pour utiliser le mode compas, assurez-vous que votre compas est calibré et que vos gains sont correctement réglés. Nous reviendrons sur ces deux modes plus tard.

## Mode GPS
Le mode GPS est un autre mode de pilotage automatique. En fait, son nom est mal choisi. Il aurait dû s'appeler mode Track. Car c'est précisément ce qu'il fait. Dans ce mode, Pypilot suit une trajectoire.

Une trajectoire est définie par un relèvement (cap prévu) et une erreur transversale. Si Pypilot est configuré en mode Suivi, il pilotera le relèvement, mais le corrigera en fonction de l'erreur transversale. Par exemple, si le relèvement est plein nord, mais que vous êtes à 50 mètres à gauche de la trajectoire, Pypilot pilotera à 010 pour revenir à la trajectoire.

Les logiciels de traçage comme OpenCPN envoient des informations de trajectoire pour les segments suivants d'une route. Si vous créez une route dans OpenCPN et l'activez, Pypilot la détectera et activera le mode Suivi, vous permettant ainsi de la sélectionner. La question de savoir si Pypilot doit être forcé de suivre une trajectoire lorsqu'il est activé ou s'il faut néanmoins choisir le mode boussole fait débat. De plus, lorsque vous passez un waypoint, Pypilot basculera vers le nouveau relèvement sans avertissement. Il est donc important d'être prudent lors de l'utilisation du mode Suivi.

## Mode Vent
Vous disposez peut-être d'une unité de mesure du vent sur votre mât qui vous indique la direction du vent par rapport à l'étrave. Si cet angle de vent n'est pas corrigé en fonction de votre vitesse, il s'agit de l'angle de vent apparent. Il est conseillé de naviguer en fonction de l'angle de vent apparent pour maintenir un angle constant. Si vous naviguez au près, par exemple, vous souhaiterez peut-être profiter des sautes de vent ou au moins éviter que votre bateau ne décroche lorsque le vent bascule vers l'étrave.

Le mode Vent de Pypilot (ou plutôt le mode Vent apparent) est activé lorsque Pypilot reçoit des informations sur l'angle de vent apparent. S'il est sélectionné, il utilisera un cap compas qui maintiendra un angle de vent moyen constant et ajustera ce cap lorsque cet angle se déplacera. Vous avez donc toujours besoin d'une IMU pour naviguer en fonction du vent.

## Mode Vent Réel
L'inconvénient de la navigation en fonction du vent apparent est que l'angle de vent apparent dépend de la vitesse du bateau et de la force du vent : plus vous naviguez vite, plus le vent apparent bascule vers l'étrave. Si la vitesse de votre bateau varie, par exemple si vous avez une mer de l'arrière et que vous surfez sur les vagues, ou si le vent souffle en rafales, il peut être préférable de naviguer en fonction de l'angle du vent réel. Pour déterminer le vent réel, vous devez Connaître la vitesse du bateau et la vitesse du vent. La vitesse du vent provient probablement de votre ventomètre, tandis que Pypilot utilise uniquement la vitesse du bateau sur la surface de l'eau (SOG) et l'attend d'un message GPS. Par conséquent, Pypilot active le mode Vent réel lorsqu'il reçoit les informations de vent et de SOG.

***

Voici les principales fonctions de Pypilot. Pour que ces fonctions de base fonctionnent, vous devez :

* étalonner vos capteurs IMU ;
* configurer correctement vos gains ; et
* fournir à Pypilot les données nautiques appropriées.

Voici les fonctions secondaires suivantes.

## Calibrage des capteurs IMU
Lorsqu'une IMU est installée sur un bateau, elle ne connaît pas sa position. Il n'existe pas de méthode prédéfinie pour la positionner : vous pouvez la placer comme vous le souhaitez, à condition qu'elle soit exempte de toute interférence magnétique. Il est donc nécessaire d'indiquer à Pypilot que le bateau est à niveau. Toutes les interfaces utilisateur proposent cette fonction : un bouton « Bateau à niveau ». N'oubliez pas de le faire. Si vous ne le mettez pas à niveau, aucun message d'erreur ne s'affichera, mais le pypilot se dirigera comme une chèvre.

Une fois que le pypilot connaît le sens de la marche, il reconnaîtra le mouvement de rotation du bateau et commencera à corréler ce mouvement avec les mesures du compas interne, construisant ainsi sa table de déviation 2D interne. Le pypilot n'a besoin que d'un demi-tour pour terminer son étalonnage initial du compas, et le signale en réinitialisant son âge d'étalonnage. Normalement, cela se fait tout seul. Il est important de noter qu'au début, le pypilot peut être un peu nerveux, mais qu'il reprendra rapidement ses esprits. Si vous ressentez une forte envie de le jeter par-dessus bord, tenez-le simplement un peu plus longtemps.

Lorsque le pypilot détecte le sens de la marche, l'étalonnage du compas 2D se poursuit. Il existe également une fonction d'étalonnage 3D, qui construit cette table de déviation pour différentes positions de tangage et de gîte. La progression de ce mécanisme d'autocalibrage est représentée en couleurs sur un globe 3D, mais pour moi, c'est de la théorie des cordes.

## Gains
Nous terminerons cela ultérieurement.

***
[Connexions de données >>>](Connexions de données)