# Ping Castle

## 1. Pré-requis

1 Domaine Active Directory
Windows 10 Entreprise Edition
Vous allez devoir installer PingCastle sur une machine cliente de votre domaine, connecter avec un compte utilisateur de ce domaine.

## 2. Installation

Dans un premier temps, rendez-vous sur la page officiel de [PingCastle](https://www.pingcastle.com/download/), dans l'onglet téléchargement, télécharger la dernière version.

Une fois le téléchargement terminée, extraire l'archive, et lancer "**PingCastle.exe**".

Voila, votre PingCastle est désormais installé et, vous avez le premier audit de votre domaine.

# Purple Knight

## 1. Pré-requis

1 Domain Active Directory
Windows 10 Entreprise Edition
Vous allez devoir installer Purple Knight sur une machine cliente de votre domaine, connecter avec un compte utilisateur de ce domaine.

## 2. Installation 
Dans un premier temps, rendez-vous sur la page officiel de [Purple Knight](https://www.purple-knight.com/request-form/), remplissez le formulaire, et attendre environ 10-15 min pour recevoir par le mail que vous avez renseignés, l'archive d'installation de Purple Knight.

Une fois l'archive téléchargée, on va enlever le "Block File", pour ce faire, ouvrez un terminal PowerShell et exécutez la commande suivante :
```PowerShell
dir -Path <emplacement de votre archive depuis la racine exemple "C:\"> -Recurse | Unblock-File
```

Vérifiez votre Politique d'exécution de mots de passes, la passez en "**RemoteSigned**" sur l'utilisateur courant au besoin.

Une fois ses deux étapes résolues, extraire l'archive, et lancer "**PurpleKnight.exe**".

Voila, votre Purple Knight est désormais installé et, vous avez le premier audit de votre domaine.

# BloodHound

## Pré-requis

1 Domain Active Directory
Windows 10 Entreprise Edition

## Installer Java

Sur ce lien : https://www.oracle.com/java/technologies/javase-jdk11-downloads.html 

## Installer Neo4j

Sur ce lien : https://neo4j.com/download-center/#community
Prendre la version community en 4.4.
Extraire les fihiers, se placer en cmd dans le dossier bin du  dossier et lancer la commande suivante  : 
```
neo4j.bat install-service
```
Puis on lance la commande  : 
```
net start neo4j
```

Aller sur le navigateur web et aller sur **http://localhost:7474/** pour changer le mot de passe.

## Installation de Bloodhound : 

Sur ce lien : https://github.com/BloodHoundAD/BloodHound/releases  
Extraire les fichiers et lancer BloodHound.exe

Télécharger SharpHound.ps1 (sur ce lien : https://github.com/BloodHoundAD/BloodHound/tree/master/Collectors)
Le lancer en powershell avec Edit puis rentrer la ligne de commande suivante : 
```
Invoke-Bloodhound -CollectionMethod All -Domain Ekoloclast.lan -ZipFileName Collect.zip
```

Une fois fait, aller sur l'interface BloodHound et cliquer sur **upload data** et sélectionner le fichier **collecte.zip**. 
Après téléchargement, Bloodhound affiche la carte du réseau. On peut modifier la carte dans les menus à gauche.
