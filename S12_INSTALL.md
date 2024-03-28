# Guide d'installation semaine 12 : 

## 1. GPO : Création type d'un GPO

### Création
Pour respecter les bonnes pratiques, nous allons crée la GPO dans le dossier "**Group Policy Object**", et nous allons la nommé avec la nomenclature suivant "**UsersXXX**" si l'on veut appliqué la GPO sur les utilisateurs et "**ComputerXXX**" si la GPO sera appliqué sur les Ordinateurs, "**XXX**" correspond à un nom explicite sur la fonction de la GPO pour exemple dans cette Install nous créons la GPO "**UsersVerrouillageSession**".
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/TutoGPO/1.png?raw=true)
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/TutoGPO/2.png?raw=true) 
### Configuration de la GPO
Toujours dans le cadre des bonnes pratiques nous allons retirer le groupe de filtrage par défaut et nous allons utilisé les groupes que nous avons crée au moment du montage de l'AD via les Scripts (Domain Computer étant une exception car nous avons besoin de ce groupe pour appliqué une GPO sur les Utilisateurs)

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/TutoGPO/3.png?raw=true)
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/TutoGPO/4.png?raw=true)
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/TutoGPO/5.png?raw=true)

Enfin vue que notre GPO sera active sur les Utilisateurs, nous désactivons les "**Paramètres de configuration Ordinateurs**"

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/TutoGPO/6.png?raw=true)
### Application des Règles pour la GPO exemple
Le but que l'on veut atteindre avec cette GPO est de verrouiller la session d'un utilisateurs si celui ci est inactif dessus depuis au moins 15 minutes.
Pour ceci nous allons allez dans le chemin ci dessous et activer les réglages activer dans l'exemple :

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/TutoGPO/7.png?raw=true)
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/TutoGPO/8.png?raw=true)
Nous commençons par activer l'écran de veille

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/TutoGPO/9.png?raw=true)

Puis nous activons le réglage obligeant l'utilisateurs à saisir son mot de passe pour enlever l'écran de veille

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/TutoGPO/10.png?raw=true)

Nous activons le délai pour l'activation de l'écran de veille et le fixons à 900 secondes soit 15 minutes

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/TutoGPO/11.png?raw=true)

Enfin  nous forçons le verrouillage de session en temps qu'écran de veille avec la ligne de commande suivantes : 
``` Batch
%windir%\system32\rundll32.exe user32.dll,LockWorkStation
```

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/TutoGPO/11B.png?raw=true)

### Application a L'OU 
Pour finir nous allons lier notre GPO fraîchement paramétrée à l'OU 01Utilisateurs pour qu'elle s'applique ainsi a tout les utilisateurs de notre AD 

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/TutoGPO/12.png?raw=true)
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/TutoGPO/13.png?raw=true)


Voila votre GPO a été crée dans les règles de l'art et elle est maintenant prête à être déployer sur votre AD ! 


## 2. GPO : Création de la GPO Mot de passe via Administrative Center

### Cheminement vers Password Setting Container
Après avoir lancé AD Administrative Center suivez le chemin suivant pour atteindre "**Password Setting Container**" 
```
<nomDuDomaine> (local) > System > Password Setting Container
```

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/TutoGPO/100.png?raw=true)

Cliquez sur "**New**" et "**Password Setting**".

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/TutoGPO/101.png?raw=true)

La fenetre de configuration apparait, nous allons remplir les choix suivants dans notre GPO :

- **Name** : Nom de la politique, dans le cas présent : PSOUsers
- **Precedence** : Valeur supérieure à 0 qui servira à faire l’arbitrage en cas de conflit entre deux PSO qui s’appliquent sur un même objet. Pensez à espacer les valeurs de vos différents PSO afin de pouvoir jouer sur les priorités facilement.
Pour cela deux règles sont à connaître :

 -  La valeur la plus faible sera prioritaire sur la valeur la plus haute.
 - En cas de priorité identique, c'est la politique avec le GUID le plus faible qui sera appliqué
 - Un PSO appliqué au niveau d’un utilisateur sera prioritaire appliqué au niveau du groupe.

Dans notre cas 1.

- **Enforce minimum password length** : chiffre entier pour définir la longueur minimale que doit faire le mot de passe. Dans notre cas 12 caractères.

- **Enforce password history** : permet de définir le nombre d’anciens mots de passe qu’un utilisateur ne peut pas réutiliser, cela le force à « inventer » un nouveau mot de passe un certain nombre de fois. Par défaut, la valeur est de 24 caractères.

- **Password must meet complexity requirements** : indiquez si oui ou non le mot de passe doit respecter ces exigences (conseillé par sécurité).

- **Enforce minimun password age** : durée de vie minimale d’un mot de passe, ce qui permet d’empêcher un utilisateur de changer successivement plusieurs fois son mot de passe. Cela pourrait lui permettre de dépasser la limite de l’historique des mots de passe et de redéfinir son mot de passe initial… Cette valeur doit correspondre à un nombre de jours. Dans notre cas 30 jours.

- **Enforce maximum password age** : même principe que le paramètre précédent, mais là pour la durée de vie maximale. Dans notre cas 180 jours.


- **Number of failed logon attempts allowed** : une fois que le nombre de tentatives autorisées sera dépassé, le compte sera verrouillé automatiquement. Cela permet d’éviter les attaques par brute force où de nombreuses tentatives seraient tentées… Dans notre cas 3 tentatives.

- **Reset failed logon attempts count after** : une fois ce délai écoulé, le nombre de tentatives échouées est remis zéro. Dans notre cas 30 minutes

- **For a duration of**  : définissez une valeur de verrouillage du compte qui sera automatiquement déverrouillé une fois le délai terminé. Dans notre cas 30 minutes.

- **Directly Applies To** : Permet d'appliqué au groupe ou utilisateurs. Dans notre cas le GRPUtilisateurs 

On obtiens donc ceci :

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/TutoGPO/104.png?raw=true)


## 3. Création de la GPO télémétrie avec le script correspondant :

Nous avons rédigé le script suivant :    
      
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/imgscriptTelemetry.JPG?raw=true)

Nous allons placer ce script dans le dossier SYSVOL avec le chemin suivant : 

```
\\win2022\SYSVOL\ekoloclast.lan\scripts\GPO-Script-Telemetry
```

Nous allons ensuite créer la GPO **COMPUTERTelemetry** en l'appliquant au groupe **GRPOrdinateurs** et à l'OU **02Ordinateurs**.
Dans edit => Computer configuration => Policies => Windows Settings => Scripts => Startup.

On rajoute le chemin de notre script dans l'onglet **Script Powershell** et on applique. 
En redémarrant le client, on voit bien dans les clés de registre que la télémétrie a été désactivée.

