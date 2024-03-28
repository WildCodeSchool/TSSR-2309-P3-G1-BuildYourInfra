
# Guide d'utilisation : 
  
## 1. Active Directory : Création automatique des Unités Organisationnelles et Groupes 

Notre but est d'avoir un script qui se base sur un csv pour créer une arborescence d'OU similaire aux différents départements de notre entreprise ainsi que de leur services.

Au début du script nous établissons une fonction qui nous permet de remplacer les caractères spéciaux et les espaces pour appliquer les bonnes pratiques de création des OU.

Nous importons dans la variable le ficher de référence dans le script pour pouvoir traiter chaque ligne et se servir des attributs "**Departement**" et "**Service**".

Nous traitons cette variable afin de supprimer tout les doublons de services et réduisons considérablement le nombre d'objets dans notre variable pour avoir une exécution du script plus rapide.

Nous créons 5 OU sans l'aide de cette nouvelle variable, ce sont des OU qui ne peuvent pas être créées avec et nécessaires au bon fonctionnement du script.  
	- L'OU 01Utilisateurs : dans lequel il y aura une arborescence d'OU fidèle au département et services de nôtre entreprise, il accueillera aussi les comptes des Utilisateurs du Domaine.  
	- L'OU 02Ordinateurs : dans lequel il y aura la même arborescence de dans l'OU 01Utilisateurs.  
	- L'OU 03Archives : Il y aura deux sous OU dans cette OU.  
		- L'OU Utilisateurs : qui aura pour but d'accueillir les comptes Utilisateurs du domaine désactivé.  
		- L'OU Ordinateurs : qui accueillera les ordinateurs en attente d'une attribution.  

Ensuite nous lancerons 4 boucles l'un derrière l'autre, elles permettent de créer les OU dans l'OU en fonction de notre variable.  

Voici un exemple de boucle :  

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/ScriptOUUSER1.png?raw=true)

## 2. Désactivation des comptes Utilisateurs et déplacement dans l'OU Archives  

Pour les besoins de notre projet, nous avons du ajouter une fonctionnalité à notre script de création d'utilisateur.  

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/ScriptADUser.png?raw=true)  

Nous avons modifié notre script pour ajouter une description à nos utilisateurs qui nous permet de trier les utilisateurs présents en semaine 9 et qui ne sont plus présents en semaine 10, et nous nous servons de cette information pour désactiver les utilisateurs non présents en semaine 10 et nous les déplaçons dans l'OU Archives.  
