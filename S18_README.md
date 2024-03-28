# TSSR-Projet3-Groupe_1-BuildYourInfra
Semaine 18
Version 1.0


## Présentation du projet :
L'objectif du projet est de réaliser un réseau dans une entreprise qui fonctionne actuellement sur une box FAI.  
Il faut d'ores et déjà prendre en compte l'évolution prochaine des effectifs suite à la fututre fusion/acquisition.

## Objectif de la semaine S10  
Cette semaine nous avions pour objectif de :   

1. AUDIT - Gérer un audit de sécurité sur l'Active Directory avec **ORADAD** (https://github.com/ANSSI-FR/ORADAD)
	1. Audit à faire
	2. Analyse des résultats et des corrections possibles
	3. Mettre en place les corrections pour augmenter le niveau de sécurité AD du CERT (https://www.cert.ssi.gouv.fr/uploads/ad_checklist.html)
2. AUDIT - Cartographier l'Active Directory avec **BloodHound** (https://github.com/BloodHoundAD/BloodHound)
	1. Détecter les chemins d'attaques possibles
	2. Corriger les failles

## Les membres du groupe :

|Nom|Rôle|Travaux effectués|
| :---: | :---: | --- |
|Thomas | SM | Installation et configuration de PingCastle et de PurpleKnight  |
|Vincent | PO | Installation et configuration de PingCastle et de PurpleKnight | 
|Fabrice | Agent actif | Installation et configuration de PingCastle et de PurpleKnight |

## Le réseau : 

| Machine | IP |
|  :---: | :---: |
| Serveur AD DC | 192.168.10.5|
| Serveur ADCORE1  | 192.168.10.80 |
| Serveur ADCORE2  | 192.168.10.85 |
| Serveur PassBolt | 192.168.10.15 |
| Client Ubuntu  | 192.168.10.20 |
| Serveur GLPI  | 192.168.10.11 |
| Serveur WSUS  | 192.168.10.50 |
| Serveur Zimbra  | 192.168.10.60 |

## Les Difficultés :
Difficultée à installer BloodHound

## Next Step : 

Rattraper les retards des semaines précédentes.
