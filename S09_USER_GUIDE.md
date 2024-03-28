# Guide d'utilisation : 
  
## Active Directory : Unités Organisationnelles et Groupes : 

Nous avons installé Active Directory (AD) sur notre server dans le cadre du développement de notre réseau informatique chez Ekoloclast (voir guide d'installation).    
  
Dans le cadre de ce projet, nous allons réaliser une arborescence détaillant les différents départements et services de l'entreprise.    
Afin de faciliter la gestion des droits et les stratégies de groupe (GPO), nous devons faire la distinction entre Unités Organisationnelles (OU) et Groupes.  
  
Une OU est une "boîte" Active Directory utilisée à l'intérieur d'un domaine. Il s'agit d'un objet, comme pour tout ce qui existe dans AD. C'est le plus petit objet présent.  
Un groupe est un ensemble d'utilisateurs, d'ordinateurs ou d'autres groupes qui peuvent être utilisés comme des ensembles de sécurité.   
On retrouvera trois sortes de groupes :   
- Universel : visible partout (pas efficace niveau sécurité).  
- Global : utilisable dans le domaine crée et dans les domaines validés par l'administrateur.  
- Local : utilisable uniquement dans le domaine créé.  

Nous utiliserons des groupes à scope Global dans ce projet.   

Pour notre arborescence, nous aurons donc des OU pour les départements et les services. Nous aurons cependant des groupes pour les fonctions dans lequels le script inscrira les différents employés en fonction du tableau excel fournit.  

Notre arborescence sera donc représentée comme ceci : 

![image](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/assets/144697101/7014e37f-1056-450f-a881-d4a6b85e30c8)

## Explication de l'utilisation du script

Notre script utilise le fichier transmis par le service RH avec les informations nécessaires sur les salariés. Nous reprenons donc l'affectation de chaque salairé afin de l'ajouter dans l'AD créé. Afin d'éviter les erreurs dues aux accents et aux caractères spéciaux nous avons une fonction qui les remplacent.

Notre script crée les utilisateurs avec un mot de passe par défaut qui devra être remplacé à la première connexion. Une vérification est également faite de l'éxistance du salarié et renvoie un message afin de ne pas générer de doublon dans ce cas. Une fois l'opération validée un message de confirmation de la création s'affiche.
