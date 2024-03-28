# Guide d'utilisation : 

## Répartition des rôles FSMO : 

Une fois les serveurs ADCORE installées en DC sur le domaine, on va répartir les rôles FSMO.   
Nous allons laisser les rôles **Maître de schéma** et **Maître d'affectation** sur le DC principal graphique car ces deux rôles ont un impact sur l'ensemble de la forêt et pas seulement le domaine.  
Nous allons transfére les rôles **PDC** et **RID Master** sur le serveur ADCORE1 car ils agissent sur le domaine et peuvent être déplacés sur un DC secondaire du domaine.  
Nous allons déplacer le rôle **Maître d'infrastructure** sur ADCORE2 car dans notre cas, n'ayant qu'un seul domaine, il n'a pas de rôle réel à jouer.  

Sur le serveur AD graphique.  
Nous allons utiliser l'outil **NTDSUTIL** :   
- Dans la barre windows, taper **run** puis rentrer **ntdsutil.exe**
- Une page en lignes de commandes apparaît
- Pour avoir de l'aide, taper **?**
- Pour passer en fsmo maintenance, taper **role**
- Taper **connections**
- Pour se connecter au serveur voulu, taper **connect to server ADCORE1** (exemple pour notre cas)
- Retourner au mode fsmo maintenance, taper **q**
- Les transferts :  
      - Transfert du rôle **Maître RID** : *transfer RID master*  
      - Transfert du rôle **PDC** : *transfer pdc*  
- Retourner en mode **connections**
- Taper **connect to server ADCORE2**
- Sortir en maintenance FSMO avec **q**
- Les transferts :  
      - Transfert du rôle **Maître d'infrastructure** : *transfer infrastructure master*  


Pour vérifier que les rôles ont bien été répartis, aller sur le panneau **cmd** et taper : 
```
NETDOM QUERY /Domain:ekoloclast.lan FSMO
```

Vous obtiendrez le résultat suivant :
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/ResultatFSMOtransfer.png?raw=true)
