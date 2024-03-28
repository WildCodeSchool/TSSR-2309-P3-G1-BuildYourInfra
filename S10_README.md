# TSSR-Projet3-Groupe_1-BuildYourInfra
Semaine 10
Version 1.0


## Présentation du projet :
L'objectif du projet est de réaliser un réseau dans une entreprise qui fonctionne actuellement sur une box FAI.  
Il faut d'ores et déjà prendre en compte l'évolution prochaine des effectifs suite à la fututre fusion/acquisition.

## Objectif de la semaine S10  
Cette semaine nous avions pour objectif de : 
  - Installer et configurer un serveur debian avec PassBolt "Logiciel de Gestionnaire de Mot de Passe"
  - Installer et configurer un deuxieme serveur AD DC pour assurer la redondance des données ainsi
  - Automatisation de la création d'OU, groupe, et Gestion des Utilisateurs partis de l'entreprise

## Les membres du groupe :

|Nom|Rôle|Travaux effectués|
| :---: | :---: | --- |
|Thomas | PO | Installation et configuration du serveur PassBolt, Script des utilisateurs |
|Vincent | Agent actif | Script des OU, installation de passbolt |
|Fabrice | SM | Installation du windows server AD Core et ajout au domaine, installation de passbolt |


### Tâches Communes : 
Réflexion sur la conception des scripts
Finalisation de la configuration de  PassBolt et de l'installation du serveur AD Core au domaine.
Configuration des IP des differente machine en concordance avec le réseaux Proxmox : 
| Machine | IP |
|  :---: | :---: |
| Serveur AD DC | 192.168.10.5|
| Serveur AD2  | 192.168.10.10 |
| Serveur PassBolt | 192.168.10.15 |
| Client Ubuntu  | 192.168.10.20 |

## Les Difficultés :
Nous n'avons pas pu finir la configuration de Passbolt en raison d'un manque de permissions sur le proxmox.  
Il a été difficile de fixer l'IP de notre serveur AD Core au début, mais nous avons par la suite réussi à le faire et à le rajouter sur notre domaine en tant que Domain Controller.
Pour le script de création d'utilisateurs, il nous a fallu du temps pour réaliser un ajout de fonctionnalité de désactivation des utilisateurs ayant quitté l'entreprise. 

## Next Step : 
- Finalisation de la configuration de Passbolt
- Modification du script d'ajout d'utilisateur
    - Changement de la méthodologie pour selectionner les utilisateurs à désactiver
    - Adapter le script pour accepter des arguments.
    - Ajout des informations complémentaires, ajout manager.
