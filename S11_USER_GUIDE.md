# Guide d'utilisation : 

## GLPI : la gestion de parc : 

GLPI est un logiciel Open-Source qui nous permet de visualiser l'ensemble des ressources de notre parc informatique. Une fois installé, il est utile pour tout suivi d'incidents et de modifications des matériels présents dans l'entreprise.   
Il ne sert cependant pas à corriger les problèmes et incidents, uniquement à les visualiser et à communiquer dessus avec les personnes concernées.  


## GLPI : le ticketing : 
  
Avec GLPI, nous pouvons mettre en place le ticketing. Lorsqu'un employé rencontre un problème informatique ou a une demande auprès du SI, il passe par GLPI et cré un ticket.    
  
Sur ce ticket, il peut choisir s'il s'agit d'un incident ou d'une demande et préciser sa gravité.  
Le ticket, une fois complété, est envoyé au SI toujours sur GLPI avec le statut **nouveau**. Un technicien va alors le prendre en charge (statut **en cours**). Une fois la solution trouvée, il va soit l'envoyer au client, soit l'envoyer à son manager pour approbation.     
Si le manager approuve, le ticket est noté comme résolu. Une fois résolu, une demande de validation est envoyé au client pour s'assurer d'avoir répondu à ses attentes. S'il le valide, le ticket passe en statut **clos**.    
C'est la fin du processus.  

Ce système permet de centraliser les demandes de tous les utilisateurs sur une plateforme de help-desk unique. GLPI prend tout de même en compte les demandes extérieures avec un onglet où l'on peut choisir si la demande vient du help-desk ou d'un mail, téléphone, vis-à-vis etc...   


## Script Création de Ordinateur

### Prérequis
Il vous faut le fichier en .CSV sur le bureau de votre serveur, avec le nom suivant : "**s11_SocieteEkoloclast.csv**" et avoir deja lancer le Script "CréationOUProjet"

### Utilisation 
Vous pouvez lancer le script sans aucune condition autre que celle précisé dans les prérequis, celui ci va importer le CSV dans une variable.  
Ensuite dans une boucle "**Foreach**", il va commencer par vérifié si l'ordinateur à créer fait bien parti de notre parc matériel, puis il vérifie si l'ordinateur existe déjà dans notre AD, enfin il va créer l'ordinateur et le placer dans l'OU auquel il doit correspondre ainsi que l'ajouter a son groupe.
