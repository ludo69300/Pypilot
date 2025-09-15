Nous avons donc un logiciel et un matériel. Comment interagir avec ce système ? Nous le faisons via une interface utilisateur. Avant de décrire les différentes options, quelques mots sur le fonctionnement interne du logiciel pypilot.

Le logiciel pypilot est composé de plusieurs modules fonctionnant simultanément en parallèle. Le logiciel peut le faire. Les modules interagissent entre eux. Comment interagissent-ils ? Ils interagissent en lisant et en écrivant des données sur un module de stockage de données commun, comme un tableau noir. Par exemple, le module compas écrit le cap actuel sur le tableau noir, le module interface utilisateur écrit le relèvement prévu sur le tableau noir, puis le module pilote automatique lit ces deux valeurs sur le tableau noir et pilote le bateau.

Le fonctionnement d'une interface utilisateur devient alors clair : elle affiche sur un écran les valeurs de certains paramètres du tableau noir, et si vous appuyez sur un bouton, l'interface utilisateur écrit les nouvelles valeurs sur ce tableau noir. Ensuite, le système combiné matériel et logiciel fonctionne en fonction des données lues sur le tableau noir.

En fait, c'est ainsi que fonctionnent toutes les interfaces utilisateur de Pypilot. Il en existe plusieurs. Elles écrivent et lisent toutes sur le même tableau noir. Elles peuvent même fonctionner en parallèle : si vous appuyez sur un bouton dans une interface utilisateur, vous voyez la même chose se produire dans l'autre. En gros, toutes ces interfaces offrent les mêmes fonctionnalités.

Passons maintenant en revue les différentes interfaces utilisateur disponibles :
* Interface navigateur
* Interface Hat
* Interface OpenPlotter (Python)
* Interface plugin OpenCPN

Tout d'abord, l'**interface navigateur**. Lorsque vous démarrez Pypilot, accédez à http://votre-serveur-pypilot et vous accédez à une interface web avec toutes les fonctions de base de Pypilot. Je vous expliquerai plus tard quelles sont ces fonctions. Pypilot devrait toujours fonctionner depuis n'importe quel ordinateur portable, tablette ou smartphone. Bien sûr, naviguer avec un téléphone à la main n'est pas optimal, mais c'est une solution de dernier recours à garder à l'esprit.

Deuxièmement, l'**interface Hat**. C'est celui qui fonctionne avec le petit écran LCD et le petit clavier. Il a été baptisé Hat car il fonctionne généralement avec un circuit imprimé à coller sur le Raspberry Pi. Ce circuit imprimé n'est pas vraiment nécessaire : il suffit de connecter les touches directement aux broches GPIO du Raspberry Pi. L'interface Hat offre l'ensemble des fonctionnalités de Pypilot via les menus de l'écran LCD. Associée à l'image de distribution TinyPilot légère et à un Raspberry Pi miniature (« pizero »), elle constitue une implémentation Pypilot autonome et très compacte.

Troisièmement, l'**interface Openplotter**. Comme indiqué précédemment, vous pouvez télécharger le système de navigation maritime OpenPlotter, qui intègre Pypilot. Une interface Python a été ajoutée spécialement pour OpenPlotter, avec des fenêtres contextuelles, etc. Cette interface Openplotter est généralement connectée au même Pypilot que celui qui fonctionne sur le Raspberry Pi. Elle dispose de toutes les fonctionnalités standard de Pypilot.

Enfin, et ce n'est pas le moins important, il existe une interface Pypilot très spéciale, qui prend la forme d'un **plugin pour OpenCPN**. OpenCPN est un logiciel de traceur de cartes nautiques, avec une carte mobile, etc. Si vous installez le plugin Pypilot dans OpenCPN, vous obtenez un bouton supplémentaire, et lorsque vous cliquez dessus, une autre interface utilisateur Pypilot apparaît. En théorie, ce plugin n'a rien à voir avec OpenCPN, car il s'agit d'une interface Pypilot supplémentaire. Elle est relativement fluide et réactive car elle a été programmée en C++, comme OpenCPN, et son emplacement pratique sous un bouton permet un démarrage rapide. Sa particularité réside dans ses fonctionnalités d'intégration spécifiques avec OpenCPN. Par exemple, la carte indique la direction vers laquelle Pypilot se dirige. Nous y reviendrons plus tard.

***

Ces quatre interfaces utilisateur ont donc toutes en commun de s'interfacer avec le système Pypilot via ce module Blackboard, intégré au logiciel Pypilot. Les véritables fonctions Pypilot sont définies par le logiciel Pypilot ; les interfaces utilisateur ne font que les rendre accessibles à vous. Il est donc temps de décrire ce que sont réellement ces fonctions Pypilot.

***
[Fonctions Pypilot >>>](Fonctions Pypilot)