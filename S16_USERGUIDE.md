# Guide d'utilisation : 

# Guide d'Utilisation de WSUS : 

Une fois WSUS configuré, on peut traiter les mises à jour. On regarde les MAJ sous le status **Failed or Needed** pour voir les MAJ nécessaires à nos ordinateurs.   

Nous pouvons les approuver ou les refuser. Si l'on approuve, on peut sélectionner pour tous les PC ou seulement ceux dans le groupe choisi.   

On peut aussi sélectionner plusieurs MAJ à la fois en filtrant dans l'onglet **Search** et en rentrant un mot clé.   

Nous pouvons aussi créer des sous-dossiers de MAJ pour les types de MAJ souhaitées (par exemple : un sous-dossier PowerShell nous montrera uniquement les MAJ PowerShell).  

# Guide d'Utilisation d'iRedMail
## 1. Accès au Serveur
Dans notre DNS nous avons renseigner un Host, qui nous permet de nous rendre sur notre interface web : "**Mail**".

Voici un tableau récapitulatif des différentes web service : 

| Adresses | Fonction |
| :--: | :--: |
| // \<ipdevotreserveur>/mail | Accès à la boite mail des utilisateurs |
| //\<ipdevotreserveur>/iredadmin | Accès à la console d'administration <br>pour l'ajouts d'utilisateurs |
## 2. L'interface Utilisateurs
Dans notre DNS nous avons renseigner un nouvel host pour éviter aux utilisateurs de retenir l'adresse IP de notre serveur iRedMail. Les utilisateurs peuvent donc accéder a leur boite mail grâce à : "**mail**" 

En allant sur "**//mail/mail/**", nous arrivons sur l'interface web permettant aux utilisateurs d'accéder à leur "boite mail".

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/UGiRedMail/Capture%20d'%C3%A9cran%202024-01-11%20104427.png?raw=true)

Après avoir renseignés leurs logins et leurs mots de passe, ils accéderont directement à leurs "**boîte de réception**", celle ci et directement utilisable.

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/UGiRedMail/Capture%20d'%C3%A9cran%202024-01-11%20104533.png?raw=true)
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/UGiRedMail/Capture%20d'%C3%A9cran%202024-01-11%20104902.png?raw=true)

On constate que le mail envoyé se trouve dans l'onglet "**Envoyés**".

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/UGiRedMail/Capture%20d'%C3%A9cran%202024-01-11%20144900.png?raw=true)

Si nous nous rendons dans la "**boîte de réception**" du destinataire, le mail apparait bien dans sa "**boîte de réception**".

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/UGiRedMail/Capture%20d'%C3%A9cran%202024-01-11%20144820.png?raw=true)

## 3. L'interface d'administration

Pour rejoindre l'interface d'administration, nous renseignons donc l'ip de notre serveur : "**192.168.10.60/iredadmin**".

Pour se connecter à l'interface web, il suffit juste de renseigner les logs du compte administration de votre serveur iRedMail, vous pouvez les trouver directement dans votre serveur dans le fichier : "**/root/iRedMail-1.6.8/iRedMail.tips**".
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/UGiRedMail/Capture%20d'%C3%A9cran%202024-01-11%20151653.png?raw=true)

Une fois connecté, pour ajouter un utilisateur, on ira dans l'onglet "**+ Add**" et on choisi l'option User.

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/UGiRedMail/Capture%20d'%C3%A9cran%202024-01-11%20144929.png?raw=true)

On renseigne les informations demandées.

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/UGiRedMail/Capture%20d'%C3%A9cran%202024-01-11%20145015.png?raw=true)
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/UGiRedMail/Capture%20d'%C3%A9cran%202024-01-11%20145152.png?raw=true)

Une fois terminé, on peut retrouver la liste des comptes utilisateurs dans "**domains and accounts**" et, on peut modifier les paramètres grâce à l'engrenage, pour par exemple, leur réattribuer un mot de passe suite à la perte de celui-ci. 

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/UGiRedMail/Capture%20d'%C3%A9cran%202024-01-11%20145250.png?raw=true)
