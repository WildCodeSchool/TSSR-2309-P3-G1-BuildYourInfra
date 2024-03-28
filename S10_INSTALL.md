# Guide d'installation : 

## Serveur Passbolt : 

### Sur le serveur : 

Pour installer un serveur Passbolt, nous allons utiliser un serveur Debian qui servira à ce rôle à l'avenir.  Nous effectuerons ces étapes en root.  

Nous allons d'abord récupérer les différents éléments suivant :   
- En premier lieu, il nous faut récupérer l'installer de passbolt :
```Bash 
wget "https://download.passbolt.com/ce/installer/passbolt-repo-setup.ce.sh"
```

- Puis, nous allons télécharger SHA512SUM pour le script d'installation. SHA512SUM permet de calculer et vérifier les empreintes numériques des fichiers chiffrés.
```Bash
wget https://github.com/passbolt/passbolt-dep-scripts/releases/latest/download/passbolt-ce-SHA512SUM.txt
```

Nous allons ensuite exécuter le script suivant : 
```Bash
sha512sum -c passbolt-ce-SHA512SUM.txt && bash ./passbolt-repo-setup.ce.sh  || echo \"Bad checksum. Aborting\" && rm -f passbolt-repo-setup.ce.sh
```

Il est temps d'installer Passbolt: 
```Bash
sudo apt install passbolt-ce-server
```

Cela va lancer une interface d'installation graphique. Nous allons maintenant en suivre les étapes : 

- La première étape demande si l'on souhaite installer une base de données MariaDB, cliquez sur oui.  
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/ImagesInstallPassbolt/passbolt1.JPG?raw=true)
  
- La seconde étape demande un nom d'utilisateur avec assez de privilèges pour l'installation, renseignez **root**.  
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/ImagesInstallPassbolt/passbolt2.JPG?raw=true)
  
- La troisième étape consiste à mettre un mot de passe.  
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/ImagesInstallPassbolt/passbolt3.JPG?raw=true)
  
- L'étape suivant demande de donner un nom d'utilisateur que Passbolt utilisera pour se connecter à la base de données.  
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/ImagesInstallPassbolt/passbolt4.JPG?raw=true)
  
- Il faut ensuite renseigner un mot de passe pour cet utilisateur.  
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/ImagesInstallPassbolt/passbolt5.JPG?raw=true)
  
- La dernière étape consiste à donner le nom que portera la base de données créée.  
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/ImagesInstallPassbolt/passbolt6.JPG?raw=true)

La partie installation sur le serveur est terminée.

### Sur le client : 

Nous pouvons désormais configurer le serveur Passbolt en interface graphique depuis un client ayant une adresse IP sur le même réseau que le serveur. Il faut que les deux machines ping.`Ici, l'addresse IP de notre serveur est 192.168.1.15.
Nous allons donc ouvrir une page internet et taper :
```
http://192.168.1.15/
```

Cela va nous amener à la page d'installation de Passbolt.  

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/ImagesInstallPassbolt/passbolt7.JPG?raw=true)  



#

## Nous avons installé notre SERVERCORE

Nous allons maintenant intégrer notre SERVERCORE dans l'AD 

#### Les prérequis sont :

- Une adresse IP fixe.
- Configurer un contrôleur de domaine existant en tant DNS sur le réseau.
- S’assurer que le ping sur le nom de domaine à rejoindre fonctionne.
- Le DNS doit être l'adresse IP du serveur DC AD existant.  

#### Saisir la ligne de code suivante :
```
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

Patienter pendant l'installation de l'AD DS.

#### Saisir cette nouvelle ligne de code :
```
Install-ADDSDomainController -DomainName "ekoloclast.lab" -InstallDns:$true -Credential (Get-Credential "ekoloclast\administratror")
```
Vous serrez invité à saisir le mot de passe du compte du domaine défini puis à confirmer l'opération.

Enfin le serveur va redémarrer.
