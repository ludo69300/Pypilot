Ceci est utile lorsque vous avez commandé un contrôleur Pypilot prêt à l'emploi et qu'il est arrivé. La question est : quelle est la prochaine étape ?

Il s'agit d'un modèle de 2019 :

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/111851755-d0c73700-8914-11eb-9e09-b276ef2d50cb.png"></p>

Voici à quoi ressemble un modèle de 2021 :

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/111852080-f1dc5780-8915-11eb-87bc-e001f3aceaf1.png"></p>

Le modèle de 2019 correspond exactement aux schémas du site pypilot.

Comme vous pouvez le voir sur la première image, lorsque vous connectez le 12 V au contrôleur de moteur sans le connecter au Raspberry Pi, une LED s'allume sur l'Arduino. Elle peut être verte ou rouge. Il s'agit de la LED d'alimentation.

Le câble à 4 fils avec connecteur étanche doit être connecté au Raspberry Pi comme suit :

Contrôleur de moteur Pypilot | Raspberry Pi
-- | --
Vcc | 3V3 broche 1
RxD | TxD broche 10
TxD | RxD broche 8
Gnd | Gnd broche 6

Veuillez noter que RxD et TxD sont croisés. Il s'agit d'une connexion point à point, et pour cela, vous devez connecter TxD à RxD et inversement. Je ne mentionnerai pas la couleur des fils ici ; vous devrez la déterminer vous-même. Si vous avez acheté une IMU sur la boutique Pypilot, elle est fournie avec un câble correspondant à celui du contrôleur de moteur Pypilot.

Une fois le Raspberry connecté, il est fort probable que la LED RxD de l'Arduino reste allumée en permanence, en plus de la LED d'alimentation. Cela signifie que l'UART n'a pas été configuré sur votre Raspberry.

Pour résoudre ce problème, accédez à Raspberry > Préférences > Configuration Raspberry Pi, cliquez sur l'onglet Interfaces, cochez Port série, décochez Console série, puis cliquez sur OK. Ne redémarrez pas encore.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/111851375-2f8bb100-8913-11eb-9cb0-82ec5b7084d2.png"></p>

Ensuite, accédez à Raspberry > Openplotter > Série, puis cliquez sur UART. L'écran deviendra gris et demandera un redémarrage :

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/111850508-c7d46680-8910-11eb-94ab-4915599029fc.png"></p>

Après le redémarrage, le voyant RxD de l'Arduino devrait être éteint. Vous pouvez ensuite exécuter pypilot sur la console et vérifier le résultat. Vous devriez constater qu'un contrôleur Arduino est détecté sur l'interface /dev/ttyAMA0. À ce stade, les voyants Rxd et Txd de l'Arduino devraient clignoter très rapidement, environ 4 fois par seconde. Toutes vos interfaces utilisateur pypilot devraient indiquer la présence d'un contrôleur de moteur.

* [Étape 15 : Comprendre motor.ino](Étape-15-Comprendre motor.ino)