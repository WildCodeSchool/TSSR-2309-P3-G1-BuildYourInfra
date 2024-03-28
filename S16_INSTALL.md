# Guide d'installation :

## Serveur WSUS : 

### Installation de WSUS : 

Sur une serveur Windows 2022 : 
Tout d'abord, ajouter le PC au domaine.  

- Ajouter des rôles et fonctionnalités
- Sélectionner "Windows Server Update Services"
- Inclure tous les outils de gestion
- Sélectionner des services de rôle "WID Connectivity" et "WSUS Services"
- Renseigner l'emplacement de stockage des mises à jour
- Installer

- Une fois l'installation terminée, cliqué en haut à gauche sur "Lancer les tâches de post-installation".


### Configuration de WSUS : 

Lancer la console "Services WSUS" : 
- "Next" pour commencer
- Ne pas cocher l'option du programme d'amélioration
- Sélectionner "Synchroniser depuis Microsoft Update"
- Ne pas cocher l'utilisation d'un proxy
- Start connecting
- Choisir les langues Français et Anglais
- Cocher "Windows Server 2019"
- Sélectionner les versions de Windows qui nous intéressent (pour nous, Windows 10, version 1903 et après, ainsi que Windows 11)
- Cocher les cases " Mise à jour critique", "Mise à jour de la sécurité", Mise à jour" et "Mise à jour de définitions"
- Cocher "Synchroniser automatiquement" et renseigner l'heure de synchronisation souhaitée
- Cocher "Commencer la synchronisation initiale"
- Terminer
- Le serveur de synchronise et la configuration de base est terminée.

### Lier les machines du domaine au serveur WSUS : 

#### Modifier la méthode d'affectation des ordinateurs : 

Sur la console WSUS : 
- "Options" à gauche
- "Ordinateurs"
- Cocher l'option "Utiliser les paramètres de stratégie de groupe ou de registre sur les ordinateurs"
- Valider


#### Créer des groupes d'ordinateurs dans WSUS : 

- Sur "Tous les ordinateurs", clic droit
- Ajouter un groupe d'ordinateurs
- Le nommer (nous avons créé chez nous "Clients","Serveurs","DC")
- On obtient une arborescence représentant les ordinateurs de notre réseau


#### Lier les PC et les serveurs à WSUS par GPO : 

Nous allons créer des GPO, une pour les paramètres en commun de nos ordianteurs et d'autres pour les paramètres spécifiques de nos Clients, DC et Serveurs, soit au total 4 GPO.
Il s'agira de GPO Ordinateurs filtrées avec le groupe GrpOrdinateurs rassemblant toutes les machines et reliées aux OU correspondantes.


#### GPO WSUS pour les paramètres communs :

Editer la GPO : 
- Configuration ordinateur
- Stratégies
- Modèles d'administration
- Composants Windows
- Windows Update  

Nous allons d'abord défninir l'adresse de notre serveur WSUS avec "Spécifier l'emplacement intranet du service de mise à jour Microsoft"
- http://srvWSUS.ekoloclast.lan:8530 (8530 = port par défaut de WSUS)
- Le mettre deux fois dans les deux premières cases

Le second paramètre se nomme "Configuration du service Mises à jour automatiques":
- Garder l'option 4 - Téléchargmeent automatique et planification des installations
- Cocher les semaines 3 et 4 car Windows publie ses MAJ courant de la seconde semaine du mois en général  

Le troisième paramètre est "Ne pas se connecter à des emplacements Internet Windows Update"
-Activer

#### GPO WSUS pour les paramètres des clients : 

Editer la GPO : 
- Configuration ordinateur
- Stratégies
- Modèles d'administration
- Composants Windows
- Windows Update

Premier paramètre : "Autoriser le ciblage côté client" : 
- Activer
- Nommer le groupe en fonction du nom donné au groupe sur le serveur WSUS (Clients puis plus tard nous ferons de même avec Serveurs et DC sur les GPO suivantes)

Second paramètre : "Désactiver le redémarrage automatique pour les mises à jour pendant les heures d'activité" :
- Activer
- Définir les heures d'activité  



#### GPO WSUS pour les paramètres des serveurs : 

Editer la GPO : 
- Configuration ordinateur
- Stratégies
- Modèles d'administration
- Composants Windows
- Windows Update

Premier paramètre : "Autoriser le ciblage côté client" : 
- Activer
- Nommer le groupe en fonction du nom donné au groupe sur le serveur WSUS (Serveurs)

Second paramètre : "Désactiver le redémarrage automatique pour les mises à jour pendant les heures d'activité" :
- Activer
- Définir les heures d'activité  



#### GPO WSUS pour les paramètres des DC : 

Editer la GPO : 
- Configuration ordinateur
- Stratégies
- Modèles d'administration
- Composants Windows
- Windows Update

Premier paramètre : "Autoriser le ciblage côté client" : 
- Activer
- Nommer le groupe en fonction du nom donné au groupe sur le serveur WSUS (DC)

Second paramètre : "Désactiver le redémarrage automatique pour les mises à jour pendant les heures d'activité" :
- Activer
- Définir les heures d'activité


#### Tester la mise en place des GPO : 

Sur un client taper dans cmd : 
```
gpupdate /force
```

Une fois le message de mise à jour approuvée affiché, redémarrer.
Une fois redémarré, rafraîchir la console WSUS, le client est maintenant visible.



## Serveur iRedMail : 


### Pré-requis
Il vous faudra :
- une VM ou un CT debian 12 de préférence, mise à jour (avec apt update/upgrade) et une connexion à internet pour récupérer le software d'installation.
- le fichier "**/etc/hostname**" doit contenir le nom de votre machine en Full Qualified Domain Name (FQDN). 
  Pour le bon déroulement de l'installation, il devra être différent de votre domaine de messagerie, exemple dans notre cas : 
```bash
#Saisir le nom de votre machine en FQDN
mail.iRedMailGH.com 
```
- le fichier "**/etc/hosts**" doit contenir le nom de votre machine en Full Qualified Domain Name (FQDN), ainsi que l'adresse Lo de votre machine. 
  Pour le bon déroulement de l'installation, il devra être différent de votre domaine de messagerie, exemple dans notre cas : 
```bash
#Saisir l'adresse Loopback, le nom de votre machine en FQDN, ainsi que "localhost"
127.0.0.1 mail.iRedMailGH.com localhost
```

- Comme vous pouvez le constater, il faut absolument que les deux champs renseignés sois identique.

### Installation de iRedMail

Une fois votre VM/CT préparée, on va récupérer "**l'installer**" via un wget :
Pensez à vérifier sur le site d'iRedMail la version actuelle.
```bash
wget https://github.com/iredmail/iRedMail/archive/refs/tags/1.6.8.tar.gz
```

Une fois l'archive récupérée, on va l'extraire avec la commande suivante : 

```bash
tar -zxf 1.6.8.tar.gz
```

l'option "**-xf**" permet d'extraire tout les fichiers présents dans l'archive.
l'option "**-z**" est obligatoire dû à l'extension "**gz**".

Une fois l'archive extraite, on va se rendre dans le dossier "**iRedMail-1.6.8**" et on va lancer le script "**iRedMail.sh**".

```bash
cd iRedMail-1.6.8
bash iRedMail.sh
```

Si vous avez bien saisie les bons prérequis, le script ce lance et un installation graphique apparaitra : 
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20152954.png?raw=true)

### Configuration

La configuration commence par un message vous signifiant que vous pouvez la suspendre à tout moment avec la commande magique "**Ctrl+C**" et vous donne un lien vers le forum d'iredmail en cas de pépin.
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20152954.png?raw=true)

Ensuite vous pouvez décidé de changer l'emplacement de stockage des mails, nous prendrons le chemin par défaut : /var/vmail.
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20153004.png?raw=true)

On continue pour le choix du serveur web, qui permettra d'utiliser l'interface web d'iRedMail, nous choisirons d'utilisé Nginx.
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20153011.png?raw=true)

Le choix de la base de donnée, comme préciser dans l'encart, choisissez la database avec laquelle vous etes le plus habitué. Nous choisirons MariaDB.
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20153018.png?raw=true)

Ici, on vous demande de renseigner le suffixe LDAP, pour nous cela sera : "**dc=ekoloclast,dc=lan**".
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20153131.png?raw=true)

Spécifiez le mot de passe que vous souhaitez utiliser.
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20153145.png?raw=true)

Ici, on va renseigner le domaine mail. "**ekoloclast.lan**".
(il est recommandé, si vous souhaitez lié votre domaine mail et AD d'utilisé exactement le même nom de domaine, cette démarche ne sera pas expliqué dans cette install).
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20153206.png?raw=true)

Choisissez le mot de passe pour l'administration de votre compte pour accéder à l'interface administrateur de votre interface web.
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20153218.png?raw=true)

Enfin, des composants optionnel. A votre bon cœur Messieurs Dame.
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20153225.png?raw=true)

Suite à cela votre serveur va effectuez l'installation, pensez à redémarrez votre serveur pour que votre serveur sois complétement fonctionnel.
