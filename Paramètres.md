La liste de paramètres suivante se limite à ceux enregistrés dans pypilot.conf.

| ap.mode | default: compass; screen: Control
-- | :--
| | Mode de pilotage automatique choisi (par exemple, compas, GPC, vent, vent réel) ; ce choix semble survivre au redémarrage.

| ap.pilot | default: basic; screen: Gain screen
-- | :--
| | Choix actuel de l'algorithme de pilotage automatique.

| ap.pilot.* | |
-- | :--
| | Voici les gains des différents pilotes automatiques. Voir [Gains](gains)

**Fonctionnalité de virement de bord**<br>
Principe de base pour un virement de bord <br>https://forum.openmarine.net/showthread.php?tid=1469&pid=7184#pid7184 :
1. Attendre le **délai de virement de bord**
2. Changer de cap pour un **angle de virement de bord** ou un angle de vent opposé
3. Déplacer le gouvernail à la **vitesse de virement de bord** jusqu'à ce qu'il atteigne la fin de course ou que le bateau atteigne le **seuil de virement de bord**

| ap.tack.angle | par défaut : 100 ; écran : Écran de configuration
-- | :--
| | Degrés. En mode vent, le calcul est effectué automatiquement à partir du cap actuel <br>https://forum.openmarine.net/showthread.php?tid=1469&pid=7184#pid7184

| ap.tack.count | par défaut : 0 ; écran :
-- | :--
| | Nombre de virements de bord effectués.

| ap.tack.delay | par défaut : 0 ; écran : écran de configuration
-- | :--
| | Combien de secondes attendre avant de virer de bord après avoir appuyé sur le bouton de virement ? <br>https://forum.openmarine.net/showthread.php?tid=1469&pid=7184#pid7184

| ap.tack.rate | par défaut : 20 ; écran : écran de configuration
-- | :--
| | Vitesse de virement de bord. L'unité est degrés/sec. <br>https://forum.openmarine.net/showthread.php?tid=1469&pid=7184#pid7184

| ap.tack.threshold | par défaut : 50 ; écran : écran de configuration
-- | :--
| | Quand revenir au filtre normal ? L'unité est le pourcentage. Généralement la moitié de l'angle de virement de bord, mais un ajustement serait utile pour éviter tout dépassement. <br>https://forum.openmarine.net/showthread.php?tid=1469&pid=7184#pid7184

| apb.xte.gain | par défaut : 300 ; écran : Client
-- | :--
| | En mode GPS (lorsque vous suivez une trace APB), corrigez le cap de la trace en fonction du XTE, multiplié par ce gain. Exemple : la trace est au nord, mais le XTE est à 0,05 à gauche. Pypilot pilotera 000 + 300 * 0,05 = 015 pour revenir à la trace.<br>https://forum.openmarine.net/showthread.php?tid=2078&pid=10805#pid10805

| imu.accel.calibration | par défaut : ; écran :
-- | :--
| |

| imu.accel.calibration.age | par défaut : - ; écran :
-- | :--
| |

| imu.accel.calibration.locked | default: false; screen: Client
-- | :--
| |

| imu.accel.calibration.points | default: ; screen:
-- | :--
| |

| imu.alignmentQ | default: ; screen:
-- | :--
| | Cela semble être le résultat de l'action « Le bateau est à niveau », et c'est un vecteur pointant vers le ciel.

| imu.compass.calibration | default: ; screen:
-- | :--
| |

| imu.compass.calibration.age | default: - ; screen: Calibration - Compass
-- | :--
| | Lorsque ce paramètre revient à 0, vous savez que votre boussole vient d'être calibrée. Je vérifie cela au début d'une sortie, je fais un demi-cercle et j'attends qu'elle se réinitialise.

| imu.compass.calibration.locked | default: false; screen: Calibration - Compass
-- | :--
| | Verrouillez l'étalonnage du compas pour éviter les mauvaises surprises.

| imu.gyrobias | default: false; screen:
-- | :--
| |

| imu.heading_offset | default: 0; screen: Calibration screen
-- | :--
| | Lorsque vous avez installé votre IMU sur votre bateau, il n'y avait aucune référence au nord. Après avoir indiqué à Pypilot que le bateau est à niveau, le cap du compas est corrigé. Ajustez-le jusqu'à ce que le cap du compas sur l'écran de contrôle de Pypilot soit égal au cap corrigé lu par le compas de votre bateau ou, en l'absence de courant ni de dérive, à votre COG.

| imu.rate | default: 10; screen: Client
-- | :--
| | Peut être réglé sur 10 ou 20 ; apparemment, cela est lié à la fréquence de mise à jour du compas/IMU. L'unité est Hz : nombre de mises à jour par seconde.<br>https://forum.openmarine.net/showthread.php?tid=1849&pid=9756#pid9756

| nmea.client | par défaut : ; écran :
-- | :--
| |

Ces paramètres résultent de la procédure d'étalonnage de la barre https://pypilot.org/wiki/doku.php?id=calibration, si vous disposez d'un capteur de retour de barre. Je les laisse ici, car cela pourrait devenir d'actualité avec l'avènement de l'algorithme pypilot « absolu ».

| rudder.nonlinearity | par défaut : 0 ; écran :
-- | :--
| |

| rudder.offset | par défaut : 0 ; écran :
-- | :--
| |

| rudder.range | par défaut : 45 ; écran : Écran d'étalonnage
-- | :--
| |

| rudder.scale | par défaut : 100 ; écran :
-- | :--
| |

| servo.amp_hours | par défaut : 0 ; écran : Statistiques
-- | :--
| | Ampères-heures cumulés utilisés par le Pypilot. Réinitialisable avec le bouton « Réinitialiser ». Peut être utilisé pour surveiller ou optimiser l'efficacité du Pypilot.

| servo.compensate_current | par défaut : false ; écran : Client
-- | :--
| | Aucune référence, ne semble pas utilisé dans le code.

| servo.compensate_voltage | par défaut : false ; écran : Client
-- | :--
| | Selon le code de servo.py, cela compense la vitesse du moteur en fonction des fluctuations de tension de la batterie (!).

| servo.current.factor | par défaut : 1 ; écran : Client
-- | :--
| | Ceci peut être utilisé pour affiner la lecture de la tension dans l'écran Statistiques. Si la tension de votre contrôleur dépasse certaines valeurs, vous obtiendrez un indicateur serveur BAD_VOLTAGE et votre pypilot ne fonctionnera pas. Si la tension est vraiment trop basse, la configuration de vos résistances de division de tension est incorrecte ; vous devrez donc repasser au fer à souder.

| servo.voltage.offset | default: 0; screen: Client
-- | :--
| | Voir ci-dessus : servo.voltage.factor

| wind.offset | default: 0; screen: Configuration screen
-- | :--
| | Aucune référence trouvée sur le forum.