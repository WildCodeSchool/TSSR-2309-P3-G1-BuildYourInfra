# Guide d'installation semaine 11 : 


## GLPI : gestion de parc : 

### Sur le serveur Débian : 

Nous allons installer GLPI sur une machine Débian 12, en root.

#### Installation des pré-requis : 

Nous allons d'abord effectuer une mise à jour : 

```Bash
apt-get update && apt-get upgrade
```

Installation d'Apache :

```Bash
apt-get install apache2 -y
```

Activation d'Apache au démarrage de la machine :

```Bash
systemctl enable apache2
```

Installation de la base de donnée. Ici nous installons MariaDB :

```Bash
apt-get install mariadb-server -y
```

#### Installation de PHP : 

Installation des dépendances : 
```Bash
apt-get install ca-certificates apt-transport-https software-properties-common wget curl lsb-release
```

Ajout du dépôt pour PHP 8.1 : 
```Bash
curl -sSL https://packages.sury.org/php/README.txt | bash -x
apt-get update
```

Installation de PHP 8.1 : 
```Bash
apt-get install php8.1
```

Installation des modules annexes :
```Bash
apt-get install php8.1 libapache2-mod-php -y

apt-get install php8.1-{ldap,imap,apcu,xmlrpc,curl,common,gd,json,mbstring,mysql,xml,intl,zip,bz2}
```

#### Installation de Mariadb : 

Nous lançons l'installation : 

```Bash
mysql_secure_installation
```

Et nous répondons par "oui" (y) à toutes les questions.

Nous allons ensuite créer la base de donner et créer l'utilisateur avec les droits : 

```Bash
mysql -u root -p
```

Dans Mariadb, on inscrit : 
```
create database glpidb character set utf8 collate utf8_bin ;
grant all privileges on glpidb.* to glpi@localhost identified by 'Azerty1*' ;
flush privileges ;
quit
```

#### Récupération des ressources de GLPI dans Github : 

On rentre la commande suivante : 

```Bash
wget https://github.com/glpi-project/glpi/releases/download/10.0.2/glpi-10.0.2.tgz
```

Puis on tape les commandes ci-après : 

```Bash
mkdir /var/www/html/glpi.ekoloclast.lan
tar -xzvf glpi-10.0.2.tgz
cp -R glpi/* /var/www/html/glpi.ekoloclast.lan/

chown -R www-data:www-data /var/www/html/glpi.ekoloclast.lan/
chmod -R 775 /var/www/html/glpi.ekoloclast.lan/
```

#### Configuration de PHP :

On va tout d'abord éditer le fichier php.ini qui est sous /etc/php/8.1/apache2/
Ensuite on va modifier les paramètres suivants :

    memory_limit = 64M
    file_uploads = on
    max_execution_time = 600
    session.auto_start = 0
    session.use_trans_sid = 0

Utiliser ctrl+w permet de rechercher plus rapidement les paramètres.

On redémarre la machine.

### Sur le client Ubuntu : 

#### Installation : 

Nous allons utiliser un client Ubuntu étant sur le même réseau que notre serveur pour configurer GLPI en interface graphique. Notre serveur est à l'adresse 192.168.10.11.

On rentre sur un navigateur : 

```
https://192.168.10.11/glpi.ekoloclast.lan
```

Nous allons arriver sur la page d'installation.  
- Sélectionner **Français** en langue
- Cliquer sur **Installer**
- Renseigner l'utilisateur et le mot de passe : ici : glpi et Azerty1*

Nous voilà sur notre GLPI. Nous avons le choix entre différents compte natifs, dont le super admin (id : glpi et mdp : glpi).
Une fois entrés dans le serveur en graphique, il nous est demandé de modifier les mots de passe de tous les comptes natifs et de supprimer le fichier **install.php** dans notre serveur glpi.  

#### Ajout à l'AD : 

Nous allons maintenant ajouter l'AD dans le GLPI.  
Il nous faut créer un utilisateur servant à la synchronisation avec AD. Cet utilisateur doit être utilisateur du domaine et ne requiert aucun droit particulier (membre du groupe « utilisateurs du domaine »). Ici notre utilisateur s’appellera « glpi-ldap ».  Cet utilisateur fera partie du groupe **UtilisateursGLPI** et de l'OU **04GLPI**

- Pour cela, nous allons sélectionner **Configuration** puis **Authentification** puis **Annuaire LDAP**.
- Nous allons cliquer sur le petit "+" et créer un annuaire.
- On clique sur **Active Directory** pour un remplissage automatique du filtre de connexion.  
- Nous configurerons le connecteur comme cela:

    **Nom**: nous donnerons ici un nom au connecteur. Nous ferons le choix de donner le nom de notre serveur Active Directory
    **Serveur par défaut**: Nous choisirons « Oui »
    **Actif**: Nous choisirons d’activer le connecteur via ce menu en choisissant « Oui ». 
    **Serveur**: Nous donnerons l’adresse IP de notre serveur, ici 192.168.10.5
    **Port**: Nous laisserons le port par défaut, le 389.
    **Filtre de connexion**: Nous mettrons les valeurs suivantes qui spécifie le lieu où chercher nos utilisateurs.

```
(&(objectClass=user)(objectCategory=person)(memberOf=CN=UtilisateursGLPI,OU=04GLPI,DC=EKOLOCLAST,DC=LAN)(!(userAccountControl:1.2.840.113556.1.4.803:=2)))
```

   **Base DN** : Nous indiquerons notre domaine active directory

```
DC=ekoloclast,DC=lan
```

   **Utiliser un compte**: Nous sélectionnerons « Oui »
   **DN du compte**: Nous indiquerons le compte que nous avons créé a destination de la synchronisation, ici glpi-ldap@ekoloclast.lan
   **Mot de passe du compte**: Nous indiquerons le mot de passe du compte de synchronisation
   **Champ de l’identifiant**: En ayant cliqué sur le bouton « Active Directory », ce champs doit être pré rempli avec la valeur « samaccountname »
   **Champ de synchronisation**: En ayant cliqué sur le bouton « Active Directory », ce champs doit être pré rempli avec la valeur « objectguid »

Enfin nous cliquerons sur le bouton **Ajouter** pour créer le connecteur.  
Une fois le connecteur crée nous allons le tester en quiquant sur **tester**. Si tout fonctionne, le test sera noté comme réussit, sinon, il faut corriger les erreurs dans la configuration du connecteur.

#### Synchronisation des utilisateurs : 

- Dans **Administration** puis **Utilisateurs**.
- Nous allons sélectionner **Liaison annuaire LDAP**.
- Puis **Importation de nouveaux utilisateurs**.
- Cliquer sur **Mode Expert**.
- **Rechercher**.

  La liste de l'ensemble des utilisateurs présents dans le filtre de recherche apparaît, nous pouvons les sélectionner puis cliquer sur **Actions** puis **Importer**.
  GLPI nous annonce que l'action a été menée à bien.

  #### Connexion des utilisateurs :

  Sur un PC client :
  - identifiant = Prenom.Nom
  - mot de passe = password
 
  On peut modifier le mot de passe à la connexion.
