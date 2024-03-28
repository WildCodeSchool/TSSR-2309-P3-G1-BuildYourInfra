# TSSR-Projet3-Groupe_1-BuildYourInfra
Semaine 17
Version 1.0


## Présentation du projet :
L'objectif du projet est de réaliser un réseau dans une entreprise qui fonctionne actuellement sur une box FAI.  
Il faut d'ores et déjà prendre en compte l'évolution prochaine des effectifs suite à la fututre fusion/acquisition.

## Objectif de la semaine S10  
Cette semaine nous avions pour objectif de :   

1. VOIP - Mettre en place le serveur de téléphonie sur IP FreePBX  
    1. Configuration de l'authentification LDAP/AD dans FreePBX (optionnel)  
    2. Création de lignes VoIP en liaison avec des comptes AD  
    3. Pouvoir valider une communication VoIP entre 2 ordinateurs clients avec 2 comptes utilisateurs AD différents  
2. PRA - Remonter les éléments HS  
    1. AD Core  
    2. GLPI  
    3. Messagerie ZIMBRA

## Les membres du groupe :

|Nom|Rôle|Travaux effectués|
| :---: | :---: | --- |
|Thomas | Agent Actif | Installation des serveurs ADCORE et répartition des rôles FSMO |
|Vincent | SM | Installation du serveur ZIMBRA | 
|Fabrice | PO | Installation du serveur GLPI |

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
Difficultés à l'installation de Zimbra.

## Next Step : 

Rattraper les retards des semaines précédentes.
