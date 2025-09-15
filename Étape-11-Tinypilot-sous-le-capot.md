Tinypilot fonctionne sous TinyCore Linux. Ce système d'exploitation diffère des distributions Linux classiques. Pour en savoir plus, nous vous conseillons de lire quelques chapitres du très bien écrit http://www.tinycorelinux.net/corebook.pdf.

- Pour un accès SSH distant, connectez-vous avec le nom d'utilisateur « tc » et le mot de passe « pypilot ».

Prenez un moment pour comprendre la principale différence entre les versions dérivées de Debian et TinyCore Linux :
- Sous Debian, les paquets sont des fichiers .deb et sont installés une fois depuis le fichier .deb vers le système de fichiers racine en lecture-écriture.
- Sous TinyCore Linux, les paquets sont des fichiers .tcz. Ils sont chargés au démarrage sur un système de fichiers en lecture seule, et des liens symboliques vers les exécutables sont créés. Le script qui charge les paquets au démarrage est le script « /opt/bootlocal.sh ». La commande qui charge les paquets .tcz est « tce-load ». Notez que /opt/bootlocal.sh s'exécute en arrière-plan, mais affiche toujours ses données sur la console. Ainsi, lorsque vous consultez la séquence de démarrage sur la console (si vous connectez un écran à votre Pi), l'invite du shell sera écrasée par la sortie de bootlocal.sh. Pypilot sera déjà en cours d'exécution, tandis que bootlocal.sh continuera de charger des « outils de développement » secondaires, ce qui pourrait afficher des messages d'erreur. Cela pourrait vous faire croire à tort qu'un problème s'est produit au démarrage de Pypilot, mais si vous consultez l'interface du navigateur, vous constaterez probablement que Pypilot est déjà opérationnel.

Prenez un moment pour comprendre comment le paquet Pypilot (pypilot.tcz !) est chargé dans Tinycore. Ouvrez /opt/bootlocal.sh et observez le code. Si vous comprenez cela, cela vous simplifiera grandement la vie :
```
# Voici le paquet pypilot :
tc@box:~$ find / -name pypilot.tcz 2>/dev/null
/mnt/mmcblk0p2/tce/optional/pypilot.tcz

# Cette ligne indique où il est « chargé » au démarrage :
tc@box:~$ grep pypilot /opt/bootlocal.sh | grep tce-load
chpst -utc tce-load -i python-serial python-RTIMULib python-ujson pypilot > /dev/null

# Après le chargement, le paquet est disponible sur ce système de fichiers en lecture seule :
tc@box:~$ df | grep pypilot
/dev/loop19 384.0K 384.0K 0 100% /tmp/tcloop/pypilot

# Voici comment accéder aux exécutables : via des liens symboliques vers le système de fichiers en lecture seule :
tc@box:~$ which pypilot
/usr/local/bin/pypilot
tc@box:~$ ls -al /usr/local/bin/pypilot
lrwxrwxrwx 1 root root 41 Jun 11 20:00 /usr/local/bin/pypilot -> /tmp/tcloop/pypilot/usr/local/bin/pypilot

Les services sont définis dans /etc/sv/
Démarrage avec sudo sv {start|stop} nom_service
Les fichiers journaux sont dans /var/log/nom_service/current

Configuration dans .pypilot, qui renvoie vers /mnt/mmcblk0p2/.pypilot
```
## Mettre à jour Pypilot sur Tinypilot
Le moyen le plus simple de mettre à jour Pypilot est de **télécharger la dernière image de Pypilot** depuis https://pypilot.org et de la graver sur la carte SD. Utilisez une nouvelle carte SD pour pouvoir revenir à la version précédente en cas de problème. Il est toutefois important de noter que la dernière image de Pypilot ne garantit pas la disponibilité des derniers correctifs logiciels de Pypilot.

Si vous avez bien compris le paragraphe précédent, vous savez déjà que mettre à jour Pypilot sur Tinypilot revient essentiellement à remplacer pypilot.tcz par une nouvelle version. Lisez attentivement :
> Pour créer un nouveau fichier pypilot.tcz, clonez Pypilot depuis GitHub et exécutez pypilot.build.

**Connexion de Pypilot à Internet** - Pour cloner Pypilot, Pypilot doit être connecté à Internet. Si vous êtes connecté en tant que client au point d'accès Wi-Fi d'OpenPlotter et qu'OpenPlotter est connecté à Internet par câble (ou, sur le bateau, par un câble USB connecté à votre smartphone), vous pouvez cloner directement depuis le TinyPilot. REMARQUE : cela fonctionne si vous avez spécifié DHCP dans les paramètres Wi-Fi du TinyPilot. Si vous avez spécifié une adresse IP fixe (adresse en mode client), vous obtiendrez le message « fatal : Impossible de rechercher github.com (port 9418) (Échec temporaire de la résolution de noms) ». C'est compréhensible, car vous n'avez pas encore spécifié de DNS ni de passerelle fixes. Assurez-vous que le fichier « nameserver 8.8.8.8 » contient le fichier « /etc/resolve.conf » et spécifiez une passerelle par défaut, par exemple « sudo route add default gw 10.10.10.1 ».

Si vous avez effectué toutes ces opérations, exécutez ce code pour mettre à jour PyPilot. Il est préférable de procéder ligne par ligne, et de capturer le résultat de chaque commande comme preuve dans un fichier texte, que vous enregistrez avec la date dans le nom du fichier.
```
cd
mkdir pypilot-update
cd pypilot-update/
git clone git://github.com/pypilot/pypilot
git clone --depth 1 git://github.com/pypilot/pypilot_data
cp -rv pypilot_data/* pypilot
cd pypilot
. pypilot.build

```
Ensuite, redémarrez votre tinypilot. `sudo reboot` fera l'affaire, en débranchant et rebranchant également l'alimentation.

**Mise à jour de tinypilot à distance** - Si la connexion de tinypilot à Internet pose problème, vous pouvez également le mettre à jour à distance depuis un Raspberry *connecté* à Internet ; Remplacez 10.10.10.3 par l'adresse IP de tinypilot :

```
git clone https://github.com/pypilot/pypilot
git clone --depth 1 https://github.com/pypilot/pypilot_data
cp -rv pypilot_data/* pypilot

scp -r pypilot/ tc@10.10.10.3:~/pypilot_update/
ssh tc@10.10.10.3 "cd pypilot_update; . pypilot.build; sudo reboot"
```

**Annuler la mise à niveau** - Dans les deux cas, vous pouvez revenir à l'ancienne version de pypilot en exécutant pypilot.build dans l'ancien répertoire source :
```
ssh tc@10.10.10.3 "cd pypilot; . pypilot.build; sudo reboot"
```

## Exécuter pypilot à l'invite
Pour exécuter pypilot à l'invite et obtenir une sortie de journalisation instantanée, arrêtez le service pypilot, puis exécutez le script pypilot. N'oubliez pas que c'est le résultat qui vous permet d'obtenir une aide ciblée lorsque vous posez une question sur le forum Pypilot :
```
tc@box:/mnt/mmcblk0p2/.pypilot$ sudo sv stop pypilot
ok: down: pypilot: 0s
tc@box:/mnt/mmcblk0p2/.pypilot$ pypilot
ERREUR lors du chargement de learning.py. Aucun module nommé tensorflow, aucun module nommé learning
ERREUR lors du chargement de learning.py. Aucun module nommé tensorflow, aucun module nommé learning
Avertissement : échec de l'inactivité du processus d'étalonnage, tentative de renouvellement
Fichier de paramètres introuvable. Utilisation des valeurs par défaut et création du fichier de paramètres
Utilisation du fichier de paramètres RTIMULib.ini
MPU9250/MPU9255 détecté à l'adresse standard
Utilisation de l'algorithme de fusion Kalman STATE4
Nom de l'IMU : MPU-925x
Calibrage du compas min/max non utilisé
Utilisation de l'étalonnage du compas ellipsoïdal
Utilisation de l'étalonnage de l'accélération
Chargement de l'étalonnage du servo dans /home/tc/.pypilot/servocalibration
AVERTISSEMENT : étalonnage du servo par défaut utilisé ! Connecté à gpsd
Pilotes chargés : ['wind', 'simple', 'basic', 'absolute']
Avertissement : échec d'ouverture du fichier spécial /dev/watchdog0 en écriture
impossible de déclencher le chien de garde
Init du MPU-925x terminée
Le servomoteur tourne trop lentement 0,0678148269653
valeur : 65535
Servo Arduino trouvé sur [u'/dev/serial/by-id/usb-1a86_USB_Serial-if00-port0', 38400]
serialprobe réussi : /home/tc/.pypilot/servodevice [u'/dev/serial/by-id/usb-1a86_USB_Serial-if00-port0', 38400]
```

***
[Étape 12 : Utilisation des outils OpenPlotter [à distance] (Étape-12- Utilisation d'openplotter-tools-à-distance)