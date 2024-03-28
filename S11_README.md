
# TSSR-Projet3-Groupe_1-BuildYourInfra
Semaine 11
Version 1.0


## Présentation du projet :
L'objectif du projet est de réaliser un réseau dans une entreprise qui fonctionne actuellement sur une box FAI.  
Il faut d'ores et déjà prendre en compte l'évolution prochaine des effectifs suite à la fututre fusion/acquisition.

## Objectif de la semaine S10  
Cette semaine nous avions pour objectif de :   

- Création d'un serveur GLPI   
	1. Synchronisation AD
	2. Gestion de parc : Inclusion des objets AD (utilisateurs, groupes, ordinateurs)
	3. Gestion des incidents : Mise en place d'un système de ticketing

- Création de GPO de sécurité  
  
- Modification fichier RH  
	1. Avec Prestataire
	2. Avec N° de PC
	3. Cases vides remplacées par "-"

- Intégration des PC du fichier dans l'AD  

## Les membres du groupe :

|Nom|Rôle|Travaux effectués|
| :---: | :---: | --- |
|Thomas | SM | Installation du serveur GLPI et configuration des utilisateurs et ticketing |
|Vincent | PO | Script modification du fichier RH |
|Fabrice | Agent actif |Script modification du fichier RH |


### Tâches Communes : 
Réflexion sur la conception des GPO
Avancée de la configuration de GLPI
Modifications des différents scripts pour répondre aux attentes
| Machine | IP |
|  :---: | :---: |
| Serveur AD DC | 192.168.10.5|
| Serveur AD2  | 192.168.10.10 |
| Serveur PassBolt | 192.168.10.15 |
| Client Ubuntu  | 192.168.10.20 |
| Serveur GLPI  | 192.168.10.11 |

## Les Difficultés :
Les modifications du script pour répondre aux besoins de la semaine ont pris du temps.  
La configuration de GLPI, son installation sur un serveur Debian au lieu d'un serveur Ubuntu et les modifications que cela a entraîné ont causé un retard.
L'ajout des utilisateurs de l'AD dans GLPI a été difficile et nous n'avons pas pour l'instant trouver comment configurer les utilisateurs tout d'un coup pour leur permettre de se connecter. Nous pouvons les configurer un par un ce qui prend beaucoup de temps.
Difficultés dans la création et la modification du code, notamment l'ajout des managers.
Problème avec une destruction de notre serveur AD principal, nous avons perdu du temps a vouloir la réparer et remonter un serveur à côté pour pouvoir poursuivre le projet.

## Next Step : 
- Finalisation de la configuration de GLPI
- Modification du script d'ajout d'utilisateur
    - Changement de la méthodologie pour selectionner les utilisateurs à désactiver
    - Adapter le script pour accepter des arguments.
    - Ajout des informations complémentaires, ajout manager.
- Mieux maîtriser les GPO pour continuer le projet sur de bonnes bases.
- Mieux comprendre les configurations de GLPI pour la connexion utilisateurs.
