
### Installation de Snort

L'installation de Snort se fait par la commande:
sudo apt install snort

Il faut maintenant confirmer ou préciser le réseau sur lequel il sera actif.
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/screenshot/snort_1.png)

Afin de ne conserver que les règles que l'on définira pour notre utilisation.
Nous allons soit supprimer, soit côter, les règles du fichier ".conf" de Snort.

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/screenshot/snort_3.png)

En profiter pour vérifier que notre réseau est bien sélectionné, toujours en fonction des besoins 
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/screenshot/snort_2.png)

### SocialPhish

Pour l'installation de SocialPhish sur votre VM Kali, il suffit juste de faire un git clone.
```Shell
git clone https://github.com/pvanfas/socialphish.git
```
Une fois le répertoire récupérer, nous allons voir comment utiliser SocialPhish dans l'UserGuide.
