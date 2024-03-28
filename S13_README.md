# TSSR-Projet3-Groupe_1-BuildYourInfra
Semaine 13
Version 1.0


## Présentation du projet :
L'objectif du projet est de réaliser un réseau dans une entreprise qui fonctionne actuellement sur une box FAI.  
Il faut d'ores et déjà prendre en compte l'évolution prochaine des effectifs suite à la fututre fusion/acquisition.

## Objectif de la semaine S10  
Cette semaine nous avions pour objectif de :   

- Créer des dossier partagés individuels pour les employés (mappage I)
- Créer des dossiers partagés par service (mappage M)
- Créer des dossiers partagés par département (mappage N)
- Mettre en place un RAID 1 sur le volume système de l'AD de Paris
- Mettre en place une sauvegarde du volume contenant les deossiers partagés dans un autre volume spécifique avec au minimum une sauvegarde par semaine.
- Créer une restriction horaire d'utilisation des machines entre 8h et 18h du lundi au vendredi


## Les membres du groupe :

|Nom|Rôle|Travaux effectués|
| :---: | :---: | --- |
|Thomas | Agent actif | Création des dossiers individuels et dossiers partagés avec les GPO, sauvegarde du volume correspondant dans une autre volume |
|Vincent | SM | Mise en place du RAID 1, création des dossiers partagés avec les GPO |
|Fabrice | PO | Politique de sauvegarde et mise en place de cette sauvegarde |


### Tâches Communes : 
Réflexion sur la conception de la sauvegarde
Création des différentes GPO pour répondre aux objectifs
| Machine | IP |
|  :---: | :---: |
| Serveur AD DC | 192.168.10.5|
| Serveur AD2  | 192.168.10.10 |
| Serveur PassBolt | 192.168.10.15 |
| Client Ubuntu  | 192.168.10.20 |
| Serveur GLPI  | 192.168.10.11 |

## Les Difficultés :
Travail répétitif pour la création des dossiers partagés entre les services. Aucune avancée sur GLPI pour rattraper le retard malgré une journée supplémentaire de recherche.

## Next Step : 

Avancer sur l'ajout des PC sur GLPI (rattraper le retard sur ce point, toujours), continuer sur les objectifs secondaires non accomplis.
