# Guide d'installation :

Nous allons ici voir l'installation d'un Active Directory sur un serveur Windows Server 2022 avec la configuration des outils utiles pour ce projet.


## Installation d'Active Directory : 

- Dans le Server Manager, cliquer sur **manage**
- On sélectionne **add rôles and features**
- On rajoute les fonctionnalités et on lance l'installation

 
### Création d'un domaine Active Directory : 

Une fois l'installation terminée :   
- Cocher **Promouvoir ce serveur en contrôleur de domaine**
- Ajouter une nouvelle forêt
- La nommer **Ekoloclast.lan**
- Mettre un mot de passe
- Installer
- On redémarre ensuite l'ordinateur.

### Création d'une Unité Organisationnelle (OU) : 

-   On sélectionne **Tools**
-   Active Directory Users and Computers
-   Sur Ekoloclast.lan ==> clic droit ==> new ==> Organizationnal Unit
-   Nommer "Utilisateurs" ==> crée tout les département et les services de la même manière en respectant l'arborescence choisie 
-   Décocher **Protect container from accidental deletion**
-   Créer l'OU.


## Connecter la VM à internet : 

Pour récupérer les fichiers sur le Google Drive via internet :   

Nous allons configurer l'adresse IP en 192.168.1.3/24.  
La passerelle sera : 192.168.1.252  
L'adresse DNS sera : 8.8.8.8 par défaut sinon 192.168.1.252  

## Pour l'utilisation du script de création des utilisateurs : 

Un script de création des utilisateurs en fonction du fichier excel fournit par le service RH est utilisé dans l'AD.    
Il faut tout d'abord enregistrer une version du fichier Excel au format .csv.  
Nous allons placer le script et le fichier CSV dans le dossier C:\  
Nous pourrons ensuite excéuter le script.  
Nous préciserons dans le scripts le chemin brut du fichier .csv
