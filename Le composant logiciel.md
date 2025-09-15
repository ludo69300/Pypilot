Le composant principal de Pypilot est le logiciel Pypilot. Ce logiciel est open source et gratuit, et pour ceux qui savent l'utiliser, sa source est disponible sur https://github.com/pypilot. Le fichier readme sur GitHub contient toutes les informations nécessaires à l'installation du logiciel à partir des sources. Cependant, il s'agit d'un sujet très technique. Si vous n'êtes pas familier avec les logiciels open source, ce document n'est pas pour vous.

### Distributions
Pour rendre les logiciels open source accessibles au plus grand nombre, des distributions prêtes à l'emploi sont créées à partir de ces derniers. Deux distributions Pypilot seront présentées dans ce manuel :
* Image OpenPlotter et
* Image TinyPilot.

**OpenPlotter** est un système de navigation maritime élaboré, basé sur du matériel Raspberry Pi et divers composants logiciels open source et gratuits. Pypilot en fait partie. Vous pouvez télécharger le système depuis le site de téléchargement d'openplotter sous forme d'image, l'enregistrer sur une carte SD, l'insérer dans un Raspberry Pi et vous disposerez de tous les composants logiciels à portée de main.

Tinypilot est également une distribution Pypilot basée sur une image, parfaitement compatible avec un petit Raspberry Pi dédié ; elle est dédiée et optimisée pour Pypilot. Elle est téléchargeable depuis le site pypilot.org. Son déploiement est légèrement plus technique, car elle est conçue pour fonctionner avec un clavier et un écran spécialement conçus, même si vous pourriez vous en passer. La bonne nouvelle, c'est que vous pouvez acheter un Tinypilot prêt à l'emploi sur la boutique Pypilot, incluant le matériel et les logiciels.

> Image : Une « image » est le contenu d'un disque complet. Plutôt que d'installer d'abord un système d'exploitation comme Linux sur un ordinateur, puis tous les autres logiciels, puis de les configurer pour qu'ils fonctionnent ensemble, une image est le contenu d'un disque sur lequel tous ces composants sont préinstallés. Il vous suffit de télécharger l'image (généralement un fichier volumineux) et de la graver sur un disque. Si cela vous semble compliqué, ce n'est pas le cas : vous pouvez être opérationnel en une demi-heure.

Pypilot fonctionne sous Linux, mais il a été développé sur Raspbian, et la plupart des implémentations fonctionnent sur Raspberry Pi 3 ou Pi 4. Tinypilot est conçu pour fonctionner sur un Pizero-W ; l'image intègre un Linux léger spécial appelé Tinycore.

![image](https://user-images.githubusercontent.com/17980560/110975408-ffb33b00-835f-11eb-90db-61a957d217cc.png)

Une autre distribution open source gratuite incluant Pypilot est LysMarine BBN (Bare Boat Necessities) Edition** https://github.com/bareboat-necessities/lysmarine_gen.

***
[Le composant matériel >>>](Le-composant-matériel)