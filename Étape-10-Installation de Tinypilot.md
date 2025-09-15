Pendant que le bateau est encore à quai, jetons un œil à TinyPilot. Les instructions d'installation de l'image sur https://pypilot.org (téléchargement) sont claires et concises, et je n'ai rien à ajouter.

Notez qu'il est mentionné ici que TinyPilot ne fonctionne pas sur Raspberry 3+ ou 4. Les premiers retours que j'ai lus de personnes ayant essayé de l'utiliser sur un Raspberry 3+ indiquaient effectivement un problème de reconnaissance de l'adaptateur sans fil. Le matériel prévu est un Raspberry Pizero-WH. Le W correspond au Wi-Fi et le H aux broches de l'embase ; aucune soudure n'est donc nécessaire.

```
pi@openplotter:~ $ wget https://pypilot.org/download.php?Down=images/tinypilot_2020_10_27.img.xz -O tinypilot_2020_10_27.img.xz
476,13 Mo 5,69 Mo/s en 81 s
pi@openplotter:~ $ xzcat tinypilot_2020_10_27.img.xz | sudo dd of=/dev/sda bs=4M
1,1 Go copié, 32 s, 32,8 Mo/s
```

Une fois la carte SD téléchargée et gravée, insérez-la et allumez-la. Le pizero est équipé d'une prise mini-HDMI, ce qui vous permettra de visualiser la séquence de démarrage sur un écran. Mais cela ne vous fera pas plaisir ; nous y reviendrons plus tard.

Au lieu de cela, comme de nombreux appareils IoT actuels, Tinypilot affiche au démarrage un point d'accès Wi-Fi avec un SSID et un mot de passe Wi-Fi connus, auquel vous pouvez connecter votre ordinateur portable pour le configurer. Il est possible de conserver le point d'accès, et Tinypilot peut alors servir de système autonome très compact. Pour cet exercice, nous allons connecter Tinypilot en tant que client Wi-Fi à notre point d'accès Wi-Fi OpenPlotter et y accéder depuis celui-ci.

* Après le démarrage, vous verrez un point d'accès Wi-Fi appelé pypilot ; il est ouvert et ne nécessite pas de mot de passe Wi-Fi. Connectez-vous-y et assurez-vous que votre ordinateur utilise DHCP.
* Accédez à http://192.168.14.1 et vous verrez l'interface du navigateur de pypilot.
* Accédez à Configuration → Configurer le Wi-Fi
* Modifiez le paramètre Maître (AP) en Client géré, le SSID en « openplotter » et le mot de passe Wi-Fi de votre point d'accès openplotter. Saisissez 10.10.10.2 comme Adresse du mode client, puis cliquez sur Envoyer.

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/111015975-a07b1800-83ab-11eb-9807-a6964a99fdd5.png"/></p>

* Après l'envoi, l'écran ne répondra plus une fois l'adresse IP modifiée, car celle-ci sera effective presque immédiatement. Vous devrez donc modifier l'adresse dans la barre d'adresse du navigateur pour continuer à utiliser l'interface. * Si vous n'avez pas spécifié d'adresse IP fixe dans le champ Adresse du mode client, recherchez l'adresse IP de votre tinypilot en saisissant ce qui suit sur votre openplotter :
```
pi@openplotter:~ $ sudo grep DHCPACK /var/log/daemon.log | tail
10 mars 22:26:46 openplotter dnsmasq-dhcp[579]: DHCPACK(wlan9) 10.10.10.159 a4:34:d9:e4:bc:d6 marco-desktop
10 mars 22:46:32 openplotter dnsmasq-dhcp[579]: DHCPACK(wlan9) 10.10.10.82 b8:27:eb:16:28:c9 box
```
Dans l'exemple ci-dessus, la dernière adresse IP attribuée était 10.10.10.82. Vous devriez donc pouvoir saisir http://10.10.10.82 dans votre navigateur pour revenir à l'interface de tinypilot.

Si vous avez spécifié une adresse IP fixe dans l'adresse du mode client boc, comme illustré sur l'image, vous n'avez pas besoin de la rechercher, car vous la connaissez déjà. L'adresse que vous spécifiez doit être comprise entre 10.10.10.2 et 10.10.10.49, sinon il pourrait y avoir des conflits avec les adresses qu'openplotter a attribuées à d'autres clients.

Vous pouvez maintenant accéder au plugin OpenCPN sur votre machine openplotter. Lorsque vous remplacez l'adresse IP de votre tinypilot dans Config --> Host, le plugin OpenCPN contrôle désormais votre tinypilot.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/110994961-e0290c00-8379-11eb-967b-e21ff14f32dd.png"/></p>

***
[Étape 11 : Tinypilot sous le capot >>>](Étape-11-Tinypilot-sous-le-capot)