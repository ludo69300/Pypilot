Vous pouvez donc installer Pypilot depuis les sources ou télécharger une distribution. Que vous faut-il de plus ?

Pypilot nécessite deux composants matériels spécifiques :
* L'unité de mouvement inertiel (IMU) ; et
* Le contrôleur Arduino.

L'IMU mesure le cap au compas et le mouvement du bateau. Comme nous le verrons plus loin, ces deux éléments (cap et mouvement) sont indissociables : ils sont indissociables et essentiels au fonctionnement de Pypilot en tant que pilote automatique. L'IMU est une puce unique (circuit intégré) qui doit être connectée au Raspberry Pi. Elle se trouve généralement sur le connecteur GPIO du Raspberry Pi. Pypilot ne peut pas fonctionner sans cette IMU ; par exemple, il est impossible de piloter avec un compas externe.

Le contrôleur Arduino fait le lien entre le logiciel Pypilot, l'actionneur (moteur) et les autres capteurs. Il s'agit toujours d'un Arduino, et généralement d'un Arduino Nano. Le logiciel qui doit fonctionner sur cet Arduino (dans le jargon Arduino : un croquis) est inclus dans la distribution logicielle pypilot, et ce croquis s'appelle motor.ino. Vous devez extraire ce fichier d'un répertoire pypilot et le graver sur l'Arduino avec le logiciel de programmation Arduino.

Le contrôleur Arduino communique avec le Raspberry Pi via une connexion série bidirectionnelle. Il peut s'agir d'une connexion série TTL ou d'un câble USB. Le protocole utilisé pour cette connexion est binaire, personnalisé et hautement optimisé. Essayer de créer une interface à ce niveau est absurde, et personne n'a jamais essayé. D'un côté, pypilot indique au contrôleur Arduino de déplacer le gouvernail, de quelle distance et à quelle vitesse. De l'autre, le contrôleur Arduino lui fournit diverses informations sur les mesures des capteurs connectés à l'Arduino, comme l'angle du gouvernail, et l'état du contrôleur lui-même, comme la tension, le courant et la température.

Le contrôleur Arduino n'est pas un contrôleur de moteur, mais il se connecte à différents types de contrôleurs de moteur. Il peut s'agir d'une interface pont en H, d'une interface de type RC ou d'un contrôleur PWM. Le choix du type de contrôleur de moteur est détaillé dans le code du croquis motor.ino, et il peut être configuré en activant ou désactivant les broches Arduino. Il peut être nécessaire de modifier le croquis.

Sur le site web pypilot.org et dans le langage courant, le contrôleur Arduino équipé d'un contrôleur de moteur pont en H est appelé « contrôleur de moteur Pypilot », et vous pouvez en acheter un en magasin. Cela pourrait laisser penser qu'il est possible de connecter n'importe quel autre contrôleur de moteur courant directement au Raspberry Pi. Ce n'est pas possible, car vous passeriez alors à côté du contrôleur Arduino et de son croquis spécifique. On pourrait dire que le logiciel Pypilot est incomplet sans le croquis Arduino.

![image](https://user-images.githubusercontent.com/17980560/110975538-2b362580-8360-11eb-83a6-9a88921e6822.png)

***
[Composant d'interface utilisateur >>>](The-User-Interface-component)