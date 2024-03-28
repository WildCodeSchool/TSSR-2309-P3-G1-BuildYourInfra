# Guide d'utilisateur : 

Semaine 14  
Version 1.0  

## Ajout de la fonction LOG dans les scripts d'ajout Users, groupes, OU et PC :

Dans chacun des scripts créés précédemment pour la création et la gestion de notre domaine AD, nous avons ajouté une fonction de journalisation afin de suivre l'ensemble des actions effectués au cours du temps.
La fonction est la suivante :   

```Powershell
Function Log
{
    param([string]$FilePath,[string]$Content)

    # Vérifie si le fichier existe, sinon le crée
    If (-not (Test-Path -Path $FilePath))
    {
        New-Item -ItemType File -Path $FilePath | Out-Null
    }

    # Construit la ligne de journal
    $Date = Get-Date -Format "dd/MM/yyyy-HH:mm:ss"
    $User = [System.Security.Principal.WindowsIdentity]::GetCurrent().Name
    $logLine = "$Date;$User;$Content"

    # Ajoute la ligne de journal au fichier
    Add-Content -Path $FilePath -Value $logLine
}
```

On définit ensuite une variable désignant le lieu où sera enregistré notre journal : 

```Powershell
#Pour le script d'ajout des PC
$LogFile="C:\Windows\Logs\Scripts\logPC"
# Pour le script d'ajout des groupes et OU
$LogFile="C:\Windows\Logs\Scripts\logOUGRP"
# Pour le script d'ajout des utilisateurs
$LogFile="C:\Windows\Logs\Scripts\logUsers"
```

A partir de là, dès que notre script effectue une action que nous estimons importante à enregistrer, nous rajoutons une ligne de journalisation après cette action. 
Cette ligne de journalisation peut prendre la forme suivante :  

```Powershell
New-ADOrganizationalUnit -Name 01Utilisateurs -path "DC=ekoloclast,DC=lan" -ProtectedFromAccidentalDeletion:$False
Log -FilePath $LogFile -Content " Création de l'OU 01Utilisateurs "
```

Dans cet exemple, nous enregistrons l'action dans le journal grâce à la ligne commençant par **Log**. **Log** est une fonction que nous avons définit avec deux paramètres. Le premier est le paramètre **Filepath** qui va déterminer où l'on enregistre l'action (ici c'est $logFile que nous avons défini plus haut). Le second est le paramètre **content** que l'on rempli manuellement pour renseigner la nature de l'action.
Dans cet exemple, nous enregistrons donc la création de l'OU 01Utilisateurs dans le fichier **C:\Windows\Logs\Scripts\logOUGRP**.

## Désactivation des Utilisateurs

```Powershell
Foreach ($row in $CSVData)
{
    $row.Prénom = RemplacementCaractereSpeciaux -String $row.Prénom
    $row.Nom = RemplacementCaractereSpeciaux -String $row.Nom   
}
$ADUsers = Get-ADUser -Filter * -Properties SamAccountName

Foreach ($ADUser in $ADUsers) 
{
    $ADUserNom =  $ADUser.Surname
    $ADUserPrenom = $ADUser.GivenName
    $ADUserLogin = $ADuser.SamAccountName
    #$ADUserSpe = $ADUser.UserPrincipalName
    
    $CSVUser = $CSVData | Where-Object { $_.Prénom -eq $ADUserPrenom -and $_.Nom -eq $ADUserNom }
    If ($ADUser.ObjectGUID -eq "5897a287-f5e0-422b-8fe9-c0c91035a363" -or $ADUser.ObjectGUID -eq "a50119a6-73b4-4ed9-a05e-badeed3236e9" -or $ADUser.ObjectGUID -eq "514ec1a1-ec9b-4fde-ac7e-6788874f511d" -or $ADUser.ObjectGUID -eq "9d0ab936-e8f4-40b4-8d0d-b4e4848187d8" )
        {
        Write-Host "Utilisateur Essentiel à ne pas toucher"
        }
    Else
        {
            If (!$CSVUser)
            {
                Write-Host "L'utilisateur $ADUserLogin n'existe pas dans le CSV. Désactivation et déplacement dans les archives."
                Set-ADUser -Identity $ADUser -Enabled:$false
                $ADUser | Move-ADObject -TargetPath "OU=Utilisateurs,OU=03Archive,DC=Ekoloclast,DC=lan"
                Log -FilePath $LogFile -Content "L'utilisateur $ADUserLogin n'existe pas dans le CSV. Désactivation et déplacement dans les archives."
            }
        }
}
```

Voici la partie du script qui nous permet la désactivation des utilisateurs de l'AD en fonction de notre CSV.    

Dans la première boucle, nous remplaçons tous les caractères spéciaux des Noms et Prénoms de notre CSV.    

Dans la deuxième boucle, on commence par exclure tous les Utilisateurs essentiels au bon fonctionnement de notre AD, dans l'ordre "Administrator, Guest, KRBTGT, et glpi-ldap"    

Ensuite nous recherchons des objets dans la variable CSVData qui correspondent aux Prénoms et aux Noms des utilisateurs de notre AD.    
Comme nous utilisons l'inversion de résultat avec le "**!**", seules les personnes qui sont présentes dans l'AD et non dans le CSV sont sorties grâce à la condition, il nous suffit juste de les désactiver puis de les déplacer dans une OU Archive, et de journaliser le tout.    

## Utilisation du logiciel de supervision PRTG : 

Pour accéder a la console web de PRTG nous utiliserons l'ip de notre machine sur laquelle nous l'avons installés : 127.0.0.1     

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/PRTG/Capture%20d'%C3%A9cran%202023-12-21%20160905.png?raw=true)

Une fois connecté, nous allons directement dans le menu en haut à gauche et sélectionnons l'onglet "**Equipements**"     

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/PRTG/Capture%20d'%C3%A9cran%202023-12-21%20155140.png?raw=true)

Dans cet onglet, on vois un résumé des différentes sonde déployé sur notre réseaux.     
En voici un extrait :     

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/PRTG/Capture%20d'%C3%A9cran%202023-12-21%20155236.png?raw=true)

Pour l'exemple, on va ajouter une sonde sur un équipement choisir au hasard, l'interface LAN de notre PFSense (192.168.10.2).    
Nous cliquons sur "**ajouter un capteur**"    

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/PRTG/Capture%20d'%C3%A9cran%202023-12-21%20155353.png?raw=true)

Une fois sur la page d'ajout de capteur, il y a un grand tableau qui nous permet d'affiner notre recherche, dans cette exemple on va choisir "**Vitesse/Perfomances**".    

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/PRTG/Capture%20d'%C3%A9cran%202023-12-21%20155405.png?raw=true)

Ce qui nous permet de trouver le capteur que l'on veut ajouter sur notre équipement : "**La Gigue du Ping**".    
Pour lancer la procédure d'ajout, il suffit juste de cliquer sur la croix en bas à droite de notre capteur.    

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/PRTG/Capture%20d'%C3%A9cran%202023-12-21%20155458.png?raw=true)

Je ne vous ferez pas l'affront de vous expliquer ce qu'est la "**Gigue du Ping**".    
Nous arrivons donc sur la deuxième étape de l'ajout de capteur, dans notre cas nous laisserons les configurations par défaut.     
Une fois votre configuration terminée, il suffit juste de cliquer sur le bouton "**Créer**"    

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/PRTG/Capture%20d'%C3%A9cran%202023-12-21%20155509.png?raw=true)

Une fois le capteur ajouté, on va l'activer. Pour cela, cliquez sur votre capteur "Gigue du Ping", une barre bleu apparaitra sur le coté droit de votre écran.    
Il vous suffira juste d'appuyez sur le bouton "**Play**"    

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/PRTG/Capture%20d'%C3%A9cran%202023-12-21%20155613.png?raw=true)

Après un laps de temps relativement court, votre capteur est lancé et actif sur votre équipement !     
A vous d'ajouter les capteurs selon les besoins de votre société.    

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/PRTG/Capture%20d'%C3%A9cran%202023-12-21%20155657.png?raw=true)
